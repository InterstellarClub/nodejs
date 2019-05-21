---
title: Debugging - Getting Started
layout: docs.hbs
---
# دليل التصحيح
سيساعدك هذا الدليل للبدء في تصحيح سكريبتات و تطبيقات الـ Node.js الخاصة بك.

# تمكين المدقق

عند بدء الـ Node.js مع تمكين `--inspect`،  يتم الانصات إلى عملية تصحيح، و افتراضيا، يتم هذا الانصات عبر المضيف و المنفذ 127.0.0.1:9229.
يتم إعطاء كل عملية إنصات رقم [UUID][] حصري.

يجب على عميل التدقيق معرفة و تحديد عنوان المضيف و رقم المنفذ، إضافة إلى الـUUID حتى يتم الاتصال.
سيبدو العنوان كاملا كما يلي: 
`ws://127.0.0.1:9229/0f2c936f-b1cd-4ac9-aab3-f63b0f33d55e`

ستستمع الـ Node.js إلى رسائل التصحيح إذا تلقت إشارة `SIGUSR1` (`SIGUSR1` غير متوفر على ويندوز)، حيث انه 
في الـ Node.js 7 و ما قبله، ينشط إستقبال هذه الإشارة واجهة برمجة التطبيقات القديمة الخاصة بالتصحيح، 
أما في الـ  Node.js 8 و ما بعده، فذلك ينشط واجهة برمجة التطبيقات الخاصة بالمدقق 



---
## تأثيرات أمنية

بما أن لدى مصحح الأخطاء وصولاً كاملاً إلى بيئة تنفيذ الـ Node.js، قد يمكن لبرمجية ضارة متصلة بهذا المنفذ أن تنفذ 
شيفرات عشوائية من خلال عملية الـ Node، ولذلك فإنه من المهم فهم التأثيرات الأمنية المحتملة التي تنتج عن إتاحة
المنفذ الخاص بمصحح الأخطاء في الشبكات العامة و الخاصة.

## إتاحة المنفذ الخاص بالتصحيح للعامة غير آمن
إذا كان لمصحح الأخطاء عنوان عام، او كان عنوانه هو 0.0.0.0، فيمكن لأي عميل قادر على الوصول لعنوان الانترنت 
الخاص بك أن يتواصل مع مصحح الأخطاء بدون أية قيود، و سيتمكن من تنفيذ برمجيات عشوائية.

إفتراضيا، تأخذ العملية `node --inspect` العنوان 127.0.0.1، ويجب عليك تحديد العنوان العام لها صراحة سواء كان 
0.0.0.0 أو غيره من العناوين إذا كنت تنوي ان تسمح بإتصالات خارجية لمصحح الأخطاء، ولكن فعل هذا قد يعرضك لمخاطر
أمنية جمة. تأكد من توظيف الجدران النارية و صلاحيات الوصول المناسبة لمنع أي تهديد أمني.

إقرأ القسم المعنون بـ [سيناريوهات تمكين تصحيح الأخطاء عن بعد](#enabling-remote-debugging-scenarios) للحصول 
على بعض النصائح حول كيفية تمكين الاتصالات عن بعد بمصحح الأخطاء بشكل آمن.

## التطبيقات المحلية تمتلك الوصول الكامل للمدقق

حتى لو قمت بربط منقذ المدقق بالعنوان 127.0.0.1 (الإفتراضي)، فإن أي تطبيقات محلية ستحصل على صلاحية الوصول الكاملة 
له. لقد تم تصميم ذلك حتى يتسنى لمصححات الأخطاء المحلية أن ترتبط به بالشكل المناسب.

## المتصفحات و مآخذ الويب و سياسة نفس الأصل

يمكن لمواقع الانترنت المفتوحة من متصفح أن تجري طلبات HTTP و webSockets تحت النموذج الأمني الخاص بالمتصفح، و يعد ضروريا
إجراء اتصال HTTP مبدئي لأجل الحصول على معرف حصري خاص بجلسة تصحيح الأخطاء. تمنع سياسة نفس الأصل المواقع من إجراء هذا الإتصال
وكتأمين من هجمات الـ [DNS rebinding](https://en.wikipedia.org/wiki/DNS_rebinding)، يقوم الـ Node.js بالتحقق من أن الرؤوس الخاصة 
بالمضيف و الخاصة بالاتصال إما تحدد عنوانا أو `localhost` أو `localhost6` بدقة.

تمنع سياسات التأمين هذه الإتصال عن طريق تحديد إسم المضيف بخادم لتصحيح الأخطاء عن بعد، لكن يمكنك إيجاد طريقة للالتفاف حول هذا بتحديد عنوان 
بروتوكول الانترنت أو باستعمال نفق ssh كما هو موصوف اسفله.

## برامج التدقيق

هناك عدة أدوات مفتوحة المصدر يمكنها الإتصال بالدقق الخاص بالـ Node.js و ما يلي هي معلومات مبدئية عنها:

#### [node-inspect](https://github.com/nodejs/node-inspect)

* مصحح أخطاء في سطر الأوامر مدعوم من مؤسسة الـ Node.js و يستعمل البروتوكول المسمى [Inspector Protocol][].
* تشحن نسخة منه مع الـ Node و يمكن استعماله بواسطة الأمر `node inspect myscript.js`.
* يمكن تثبيت آخر نسخة منه بشكل مستقل (`npm install -g node-inspect` على سبيل المثال) و يمكن استعمالها  بواسطة الأمر `node inspect myscript.js`.

#### <span dir="ltr">[Chrome DevTools](https://github.com/ChromeDevTools/devtools-frontend) 55+</span>

* **الطريقة الأولى**: قم بفتح  العنوان `chrome://inspect` في أي متصفح مبني على Chromium. قم بالضغط على زر Configure و تأكد 
من أن المضيف و المنفذ في القائمة.
* **الطريقة الثانية**: قم بنسخ `devtoolsFrontendUrl` من الناتج عن `/json/list` (أنظر أعلاه) أو النص التلميحي الناتج عن --inspect 
و قم بلصقه في Chrome.

#### <span dir="ltr">[Visual Studio Code](https://github.com/microsoft/vscode) 1.10+</span>

* في قائمة تصحيح الأخطاء، إضغط على ايقونة الإعدادات لفتح `.vscode/launch.json`.
  قم باختيار "Node.js" للتثبيت الأولي. 

#### [Visual Studio](https://github.com/Microsoft/nodejstools) 2017
* قم باختيار "Debug > Start Debugging" من القائمة أو قم بالضغط على F5.
* [خطوات تفصيلية بالإنجليزية](https://github.com/Microsoft/nodejstools/wiki/Debugging)

#### [JetBrains WebStorm](https://www.jetbrains.com/webstorm/) 2017.1+ و منتجات JetBrains الأخرى

* قم بإنشاء إعدادات تصحيح جديدة خاصة بالـ Node.js و اضغط على Debug. سيتم استعمال `--inspect` افتراضياً بالنسبة للنسخ 7 فما فوق.
  لإلغاء ذلك، قم بإلغاء تمكين `js.debugger.node.use.inspect` في السجل الخاص بالبرنامج.

#### [chrome-remote-interface](https://github.com/cyrus-and/chrome-remote-interface)
* مكتبة تسهل التواصل بأطراف اتصال بروتوكول التدقيق.

#### [Gitpod](https://www.gitpod.io)

* قم بإنشاء إعدادات تصحيح الأخطاء الخاصة بالـ Node.js من `Debug` أو قم بالضغط على F5. هنا [خطوات تفصيلية بالإنجليزية](https://medium.com/gitpod/debugging-node-js-applications-in-theia-76c94c76f0a1)

---

## خيارات سطر الأوامر


The following table lists the impact of various runtime flags on debugging:

<table cellpadding="0" cellspacing="0">
  <tr><th>Flag</th><th>Meaning</th></tr>
  <tr>
    <td>--inspect</td>
    <td>
      <ul>
        <li>Enable inspector agent</li>
        <li>Listen on default address and port (127.0.0.1:9229)</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>--inspect=<i>[host:port]</i></td>
    <td>
      <ul>
        <li>Enable inspector agent</li>
        <li>Bind to address or hostname <i>host</i> (default: 127.0.0.1)</li>
        <li>Listen on port <i>port</i> (default: 9229)</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>--inspect-brk</td>
    <td>
      <ul>
        <li>Enable inspector agent</li>
        <li>Listen on default address and port (127.0.0.1:9229)</li>
        <li>Break before user code starts</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>--inspect-brk=<i>[host:port]</i></td>
    <td>
      <ul>
        <li>Enable inspector agent</li>
        <li>Bind to address or hostname <i>host</i> (default: 127.0.0.1)</li>
        <li>Listen on port <i>port</i> (default: 9229)</li>
        <li>Break before user code starts</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td><code>node inspect <i>script.js</i></code></td>
    <td>
      <ul>
        <li>Spawn child process to run user's script under --inspect flag;
            and use main process to run CLI debugger.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td><code>node inspect --port=xxxx <i>script.js</i></code></td>
    <td>
      <ul>
        <li>Spawn child process to run user's script under --inspect flag;
            and use main process to run CLI debugger.</li>
        <li>Listen on port <i>port</i> (default: 9229)</li>
      </ul>
    </td>
  </tr>
</table>

---

## سيناريوهات تمكين تصحيح الأخطاء عن بعد <a name="enabling-remote-debugging-scenarios">

We recommend that you never have the debugger listen on a public IP address. If
you need to allow remote debugging connections we recommend the use of ssh
tunnels instead. We provide the following example for illustrative purposes only.
Please understand the security risk of allowing remote access to a privileged
service before proceeding.

Let's say you are running Node on remote machine, remote.example.com, that you
want to be able to debug. On that machine, you should start the node process
with the inspector listening only to localhost (the default).

```bash
$ node --inspect server.js
```

Now, on your local machine from where you want to initiate a debug client
connection, you can setup an ssh tunnel:

```bash
$ ssh -L 9221:localhost:9229 user@remote.example.com
```

This starts a ssh tunnel session where a connection to port 9221 on your local
machine will be forwarded to port 9229 on remote.example.com. You can now attach
a debugger such as Chrome DevTools or Visual Studio Code to localhost:9221,
which should be able to debug as if the Node.js application was running locally.

---

## Legacy Debugger

**The legacy debugger has been deprecated as of Node 7.7.0. Please use --inspect
and Inspector instead.**

When started with the **--debug** or **--debug-brk** switches in version 7 and
earlier, Node.js listens for debugging commands defined by the discontinued
V8 Debugging Protocol on a TCP port, by default `5858`. Any debugger client
which speaks this protocol can connect to and debug the running process; a
couple popular ones are listed below.

The V8 Debugging Protocol is no longer maintained or documented.

#### [Built-in Debugger](https://nodejs.org/dist/latest-v6.x/docs/api/debugger.html)

Start `node debug script_name.js` to start your script under Node's builtin
command-line debugger. Your script starts in another Node process started with
the `--debug-brk` option, and the initial Node process runs the `_debugger.js`
script and connects to your target.

#### [node-inspector](https://github.com/node-inspector/node-inspector)

Debug your Node.js app with Chrome DevTools by using an intermediary process
which translates the Inspector Protocol used in Chromium to the V8 Debugger
protocol used in Node.js.

<!-- refs -->

[Inspector Protocol]: https://chromedevtools.github.io/debugger-protocol-viewer/v8/
[UUID]: https://tools.ietf.org/html/rfc4122
