---
title: Anatomy of an HTTP Transaction
layout: docs.hbs
---

# التركيبة البنيوية لمُعَامَلَة HTTP

الغاية من هذا الدليل هو مَنَح معارف قوية لعمل Node.js في معالجة HTTP. لنفرض أنك تعرف 
بشكل عام كيف تعمل طلبات HTTP بغض النظر عن اللغة أو بيئة البرمجة وسنفرض أيضا على دِرَايَة 
 بقليلا من Node.js [`EventEmitters`][] و [`Streams`][]. إذا لم تكن فِعْلاً على دِرَايَة بهم وإنه 
 الجَدِير قيام بقراءة سريعة من خلال توثيقات واجهة برمجة التطبيقات (API) لكل منهم.


## إنشاء الخادم

أي تطبيق خادم الويب node وفي نقطة ما لإنشاء كائن خادم الويب، يتم ذلك
 بإستعمال [`createServer`][].

```javascript
const http = require('http');

const server = http.createServer((request, response) => {
  // السحر يحدث هنا!
});
```

هذه الدالة تمرر داخل [`createServer`][] وهذا يدعى واحدة لكل طلب HTTP وهذا ما يجعله
مُقَابِلَ هذا الخادم ولذا يدعى معالج الطالب، في الحقيقة كائن الخادم [`Server`][] مرجع
بواسطة [`createServer`][] هو مُصدِر الحدث [`EventEmitter`][] و ما لدينا هنا فقط إختزال 
لإنشاء كائن الخادم `server` وبعدها إضافة المستمع لاحقا.


```javascript
const server = http.createServer();
server.on('request', (request, response) => {
  // نفس النوع من السحر يحدث هنا!
});
```

عندما طلب HTTP يضرب الخادم، node يستدعي دالة معالجة الطالب مع القليل من الكائنات
المتاحة لتعامل مع المُدَاوَلَةٌ الطلب `request` و الجواب `response`، سوف نجلبهم قَرِيباً.

في التقدم الفعلي لطلب الخادم، طريقة الإستماع [`listen`][] تحتاج لإستدعاء كائن الخادم 
`server` في أغلب الحالات، كل ماتحتاج لتمريره للمستمع `listen` هو رقم المنفذ "port" الذي
تريد الخادم الإستماع إليه، يوجد بعض الخيارات الاخرى أيضا لذا راجع المرجع [API reference][].


## طريقة 'Method' و رابط 'URL' و رؤوس 'Headers'

عند معالجة الطلب أول حاجَة ربما تود القيام بها هي تفقد الطرق و الرابط URL، لذا
هذه الإجراءات المُلاَئِمة يمكن إتخاذها. Node جعل هذه متعبة نسبيا بوضع خَواصُّ المعالجة داخل
كائن الطلب `request`.

```javascript
const { method, url } = request;
```
**ملاحظة:** كائن الطلب `request` هو مثيل لرسالة القادمة [`IncomingMessage`][].

الطريقة `method` هنا ستكون دائما HTTP method/verb عاديا و `url` هو الرابط بدون خادم و
البروتوكول أو المنفذ. لرابط نموذجي هذا يعني أن كل شئ بعد و مُتَضَمّن الخط مائلة للأمام "/" الثالث.

الرؤوس هي أيضا ليست بعيدة جدا، هم يملكون كائن في الطلب `request` يدعى الرؤوس `headers`.

```javascript
const { headers } = request;
const userAgent = headers['user-agent'];
```

من المهم أن نذكر هنا أن جميع الرؤوس تًظهر الأحرف الصغيرة فقط وبِغَضّ النّظَرِ عن كيف ما 
أرسلها العميل. هذا يسهل عملية تحليل الرؤوس لأي غرض محتمل.

إذا تم تكرار بعض الرؤوس وبعدها هذه القيم إعادة الكتابة عليها أو سلاسل مَوْصُولة بفاصلة
إسْتِنَاداً إلى الرأس، في بعض الحالات يمكن أن يكون مشكل لذا الروؤس الخام [`rawHeaders`][]
هي أيضا متاحة.

## طلب الجسم (Request Body)

<!-- When receiving a `POST` or `PUT` request, the request body might be important to
your application. Getting at the body data is a little more involved than
accessing request headers. The `request` object that's passed in to a handler 
implements the [`ReadableStream`][] interface. This stream can be listened to or
piped elsewhere just like any other stream. We can grab the data right out of
the stream by listening to the stream's `'data'` and `'end'` events. -->
عند تلقي طلب من `POST` أو `PUT`، جسم الطلب ربما يكون مهماً لتطبيقك

الأجزاء الباعثة في كل `'data'` والحدث هو [`Buffer`][]. إذا كنت تعرف أنها
ستكون سِلْسِلَة من البيانات، أفضل شئ تفعله هو تجميع البيانات في مصفوفة
وبعدها في `'end'`، الرصفهم وتحويلهم إلى نصوص.

```javascript
let body = [];
request.on('data', (chunk) => {
  body.push(chunk);
}).on('end', () => {
  body = Buffer.concat(body).toString();
  // at this point, `body` has the entire request body stored in it as a string
});
```

<!-- > **Note:** This may seem a tad tedious, and in many cases, it is. Luckily,
there are modules like [`concat-stream`][] and [`body`][] on [`npm`][] which can
help hide away some of this logic. It's important to have a good understanding
of what's going on before going down that road, and that's why you're here! -->

## حاجَة سريعة حول الأخطاء

<!-- Since the `request` object is a [`ReadableStream`][], it's also an
[`EventEmitter`][] and behaves like one when an error happens. -->

<!-- An error in the `request` stream presents itself by emitting an `'error'` event
on the stream. **If you don't have a listener for that event, the error will be
*thrown*, which could crash your Node.js program.** You should therefore add an
`'error'` listener on your request streams, even if you just log it and
continue on your way. (Though it's probably best to send some kind of HTTP error
response. More on that later.) -->

```javascript
request.on('error', (err) => {
  // This prints the error message and stack trace to `stderr`.
  console.error(err.stack);
});
```

هنالك طرق أخرى لمعالجة الأخطاء [handling these errors][] مثل بعض ملخصات و
الأدوات لكن كن دائمًا على دراية بأن الأخطاء يمكن أن تحدث وستحدث و ستقوم
بالتعامل معهم.

## على ماذا حصلت لِحَدّ الآن

في هذه النقطة لقد غطينا إنشاء خادم و إنْتِزاع الطرق و الروابط الروؤس و الجسم من 
الطلبات. عندما نضع كل ذلك معا رُبّمَا ستظهر كما هذه:

```javascript
const http = require('http');

http.createServer((request, response) => {
  const { headers, method, url } = request;
  let body = [];
  request.on('error', (err) => {
    console.error(err);
  }).on('data', (chunk) => {
    body.push(chunk);
  }).on('end', () => {
    body = Buffer.concat(body).toString();
    // At this point, we have the headers, method, url and body, and can now
    // do whatever we need to in order to respond to this request.
  });
}).listen(8080); // Activates this server, listening on port 8080.
```

إذا شغلنا هذا المثال سنكون قادرين على *تلقي* الطلبات، لكن لا *نرد* عليهم،
في حَقِيقَةِ الأمْر إذا عَرَض  هذا في المتصفح، طلبك سيكون خارج المهلة ولاشئ يعاد
.إرساله للعميل


حتى الآن نحن لم نلمس كائن الجواب `response` كلياً. أَيّما في ما ذاك لـ[`ServerResponse`][]
و مِمّ هو [`WritableStream`][]. أنه يحتوي على العديد من الطرق المفيدة لإرسال البيانات
الراجعة لعميل و سنقوم بتغطية ذلك لاحقا.

## رمز الحالة HTTP

إذا كنت لا تُبالي في إعداده، رمز الحالة لـHTTP سيكون دائما في الروؤس 200،
طبعاً ليس في كل جواب يعلمك به و في بعض النقاط حتما ستريد إرسال رمز حالة
مختلف، للقيام بهذا يمكنك تعيين خاصية `statusCode`.

```javascript
response.statusCode = 404; // أخبر العميل أنه لم يتم العثور على المصدر.
```

هناك بعض الاختصارات الأخرى لهذا ، كما سنرى قريبًا.

## إعداد جواب رؤوس

يتم تعيين الرؤوس من خلال طريقة مناسبة تسمى [`setHeader`] [].

```javascript
response.setHeader('Content-Type', 'application/json');
response.setHeader('X-Powered-By', 'bacon');
```

عند ضبط الرؤوس على الجواب، في الحالة التي تكون غير مدرك لأسمائهم.
إذا عينت الروؤس بشكل متكرر القيمة الأخيرة التي هي القيمة التي ترسل.

## مُرفق إرسال بيانات الرأس

الطرق لضبط الروؤس و رمز الحالة والتي سبق أن تناقشنا بإعتبرك أنك تستخدم "الروؤس المضمنة" 
"implicit headers". هذا يعني أنك مُعْتَمِد على node للإرسال الروؤس لك في الوقت الحالي قبل البدء
في إرسال بيانات الجسم.

إذا أردت يمكنك *تصريح* بكتابة الروؤس لتدفق الجواب، للقيام بهذا يوجد طريقة تدعى [`writeHead`][]
التي تكتب رمز الحالة و الروؤس إلى التدفق.

```javascript
response.writeHead(200, {
  'Content-Type': 'application/json',
  'X-Powered-By': 'bacon'
});
```

بمجرد تعيينك الروؤس (سواء ضمنيًا أو صريحًا)، أنت مستعد للبدء في إرسال بيانات الجواب.

## إرسال جواب الجسم

بِما أَنَّ كائن الجواب `response` هو تدفق قابل للكتابة [`WritableStream`][]، كتابة جسم الجواب 
خارجيا للعميل هي فقط مَسْألَة للإستخدام طرق التدفق الإعتيادية.


```javascript
response.write('<html>');
response.write('<body>');
response.write('<h1>Hello, World!</h1>');
response.write('</body>');
response.write('</html>');
response.end();
```

دالة النهاية `end` في التدفقات يمكن ان تأخذ في بعض البيانات الإختيارية لإرسالها 
كأخر حرف من البيانات في التدفقات لذا يمكن تبسيطها في المثال الأعلى.

```javascript
response.end('<html><body><h1>Hello, World!</h1></body></html>');
```

**ملاحظة:** من المهم تعيين الحالة و الروؤس *قبل* البدء بكتابة أقسام من البيانات
للجسم وهذا يبدو منطقيا. منذ متى تأتي النص  قبل الرؤوس في جوابات HTTP.

## حاجَة أخرى سريعة حول الأخطاء


تدفق الجواب `response` يمكن أن يبعث حالة الأخطاء `'error'` و بعض النقاط ستتعامل معه أيضا.
جميع النصائح لتدفق أخطاء الطلب `request` لاتزال تطبق هنا.

## لنضعهم جميعا مع بعض

الآن بعد أن تعلمنا عن عمل جوابات HTTP، لنقم بوضعهم كلهم معاً.
بناء على المثال السابق سنقوم بإنشاء خادم يعيد إرسال جميع البيانات التي إرسلها
إلينا من قِبل المستخدم وسنقوم بتنسيق البيانات على شكل JSON بإستعمال `JSON.stringify`.

```javascript
const http = require('http');

http.createServer((request, response) => {
  const { headers, method, url } = request;
  let body = [];
  request.on('error', (err) => {
    console.error(err);
  }).on('data', (chunk) => {
    body.push(chunk);
  }).on('end', () => {
    body = Buffer.concat(body).toString();
    // BEGINNING OF NEW STUFF

    response.on('error', (err) => {
      console.error(err);
    });

    response.statusCode = 200;
    response.setHeader('Content-Type', 'application/json');
    // Note: the 2 lines above could be replaced with this next one:
    // response.writeHead(200, {'Content-Type': 'application/json'})

    const responseBody = { headers, method, url, body };

    response.write(JSON.stringify(responseBody));
    response.end();
    // Note: the 2 lines above could be replaced with this next one:
    // response.end(JSON.stringify(responseBody))

    // END OF NEW STUFF
  });
}).listen(8080);
```

## مثال لتردد خادم

لنسهل المثال السابق لإنشاء خادم ترددي بسيط، أَيّ يرسل مهما كانت البيانات فقط والتي أستلمت
من توا من الجواب. كل ما نريد فعله هو أخذ طلب التدفق و كتابة البيانات في جواب التدفق وهو
مماثل ما فعلنا في السابق.


```javascript
const http = require('http');

http.createServer((request, response) => {
  let body = [];
  request.on('data', (chunk) => {
    body.push(chunk);
  }).on('end', () => {
    body = Buffer.concat(body).toString();
    response.end(body);
  });
}).listen(8080);
```

الآن لنقم تطويع هذه، نريد إرسال فقط تردد وفقا لشروط لمتبعة:

طريقة الطلب هي POST.
الرابط 'URL' هو `/echo`

في حالة أخرى نحن نريد تبسيط الرد مع 404.

```javascript
const http = require('http');

http.createServer((request, response) => {
  if (request.method === 'POST' && request.url === '/echo') {
    let body = [];
    request.on('data', (chunk) => {
      body.push(chunk);
    }).on('end', () => {
      body = Buffer.concat(body).toString();
      response.end(body);
    });
  } else {
    response.statusCode = 404;
    response.end();
  }
}).listen(8080);
```


>**ملاحظة:** عند التحقق من الرابط URL بهذه الطريقة، نحن نقوم بشكل من التوجيه "routing".
 ويوجد أشكال أخرى للتوجيه بسيطة مثل دالة `بَدَّالَةٌ` `switch` أو معقدة كما في أدوات مثل 
 [`express`][]. إذا كنت نبحث على شئ يقوم بالتوجيه ولاشئ أخر جرب [`router`][].

رائع! الآن نستقر على تبسيط هذا وتذكر كائنات الطلب `request` هي تدقف قابل للقراءة 
[`ReadableStream`][] و كائنات الجواب `response` هي تدفق قابل للكتابة [`WritableStream`][].
وهذا يعني أنه يمكننا إستخدام مَجْرىً لتوجيه البيانات من واحدة لأخرى. وهذا تماما مانريده 
من خادم إرتدادي!

```javascript
const http = require('http');

http.createServer((request, response) => {
  if (request.method === 'POST' && request.url === '/echo') {
    request.pipe(response);
  } else {
    response.statusCode = 404;
    response.end();
  }
}).listen(8080);
```

أجل التدفقات!

نحن لم ننتهي بعد على الرغم كما ذكرنا في عدة مرات في هذا الدليل، الأخطاء واردة
 و نحتاج التعامل معها.

لتعامل مع الأخطاء في طلب التدقف، وكذا مخرج الخطأ في `stderr` و إرسال رمز الحالة 404
تدل على أن طلب سيء `Bad Request` ليس كما التطبيق في الحقيقي على أية حال، نود تفحص 
الخطأ لمعرفة ماهو رمز الحالة الصحيح وما ستكون الرسالة كالعادة  في الأخطاء،يجب عليك مراجعة 
توثيقة `الخطأ` [`Error` documentation][].

<!-- On the response, we'll just log the error to `stderr`. -->


```javascript
const http = require('http');

http.createServer((request, response) => {
  request.on('error', (err) => {
    console.error(err);
    response.statusCode = 400;
    response.end();
  });
  response.on('error', (err) => {
    console.error(err);
  });
  if (request.method === 'POST' && request.url === '/echo') {
    request.pipe(response);
  } else {
    response.statusCode = 404;
    response.end();
  }
}).listen(8080);
```


لقد قمنا الآن بتغطية أغلب الأساسيات مُعَالَجَة طالبات HTTP وفي هذه المرحلة من المفترض 
تقدر على:

تجسيد HTTP مع دالة معالجة الطلب و لإستماع للمنفذ.
الحصول على الرؤوس و الرابط 'URL' و طرق و بيانات الجسم من كائنات `request`.
صنع قرارات التوجيه إستنادا إلى الرابط و / أو  بيانات أخرى في كائنات `request`.
إرسال الرؤوس و رموز حالات HTTP و بيانات الجسم بواسطة كائنات `request`.
نقل البيانات من كائنات `request` وإلى كائنات `response`.
تعامل مع أخطاء التدفق  في حالتي تدقفات `request` و `response`.
من هذه الأساسيات، خوادم Node.js HTTP في العديد من حالات نَوْعِيّة يمكن إنشاؤه. يوجد 
الكثير مثل هذه الأشياء مزودة بـAPIs لذا تأكد من قرأة التوثيقات الAPI منها 
[`EventEmitters`][] و [`Streams`][] و [`HTTP`][].


[`EventEmitters`]: https://nodejs.org/api/events.html
[`Streams`]: https://nodejs.org/api/stream.html
[`createServer`]: https://nodejs.org/api/http.html#http_http_createserver_requestlistener
[`Server`]: https://nodejs.org/api/http.html#http_class_http_server
[`listen`]: https://nodejs.org/api/http.html#http_server_listen_port_hostname_backlog_callback
[API reference]: https://nodejs.org/api/http.html
[`IncomingMessage`]: https://nodejs.org/api/http.html#http_class_http_incomingmessage
[`ReadableStream`]: https://nodejs.org/api/stream.html#stream_class_stream_readable
[`rawHeaders`]: https://nodejs.org/api/http.html#http_message_rawheaders
[`Buffer`]: https://nodejs.org/api/buffer.html
[`concat-stream`]: https://www.npmjs.com/package/concat-stream
[`body`]: https://www.npmjs.com/package/body
[`npm`]: https://www.npmjs.com
[`EventEmitter`]: https://nodejs.org/api/events.html#events_class_eventemitter
[handling these errors]: https://nodejs.org/api/errors.html
[`domains`]: https://nodejs.org/api/domain.html
[`ServerResponse`]: https://nodejs.org/api/http.html#http_class_http_serverresponse
[`setHeader`]: https://nodejs.org/api/http.html#http_response_setheader_name_value
[`WritableStream`]: https://nodejs.org/api/stream.html#stream_class_stream_writable
[`writeHead`]: https://nodejs.org/api/http.html#http_response_writehead_statuscode_statusmessage_headers
[`express`]: https://www.npmjs.com/package/express
[`router`]: https://www.npmjs.com/package/router
[`pipe`]: https://nodejs.org/api/stream.html#stream_readable_pipe_destination_options
[`Error` documentation]: https://nodejs.org/api/errors.html
[`HTTP`]: https://nodejs.org/api/http.html
