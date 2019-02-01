---
layout: about.hbs
title:  مجموعات العمل
---
# مجموعات العمل الأساسية
<!-- Information here should mostly mirror: https://github.com/nodejs/node/blob/master/WORKING_GROUPS.md -->

يتم إنشاء مجموعات العمل الأساسية من قبل لجنة التوجيه التقني
[لجنة التوجيه التقني (TSC)](https://github.com/nodejs/TSC/blob/master/TSC-Charter.md).

## مجموعات العمل الحالية

* [addon API](#addon-api)
* [القياس](#القياس)
* [البناء](#البناء)
* [التشخيص](#التشخيص)
* [دوكر](#دوكر)
* [الإدماج](#الإدماج)
* [i18n](#i18n)
* [الإصدارات](#الإصدارات)
* [الحماية](#الحماية)
* [التدفق](#التدفق)
* [الموقع](#الموقع)

### [addon-api](https://github.com/nodejs/nan)

ان مجموعة <span dir="rtl">Addon API<span> مسؤولة عن صيانة مستودع مشروع NAN و الحزم التابعة له تحت مسمى _nan_ على مدير حزم النود. يوفر مشروع NAN طبقة مجردة للمؤلفين الاصليين لاضافة النود جي اس، و ذلك عبر المساعدة في كتابة شفرة برمجية متوافقة مع عدة اصدارات نشطة من النود جي اس و V8 و libuv.
تشمل مسؤوليات هذه المجموعة:
* صيانة مستودع [NAN](https://github.com/nodejs/nan) على الـGitHub، بما في ذلك الشفرة البرمجة، والمشاكل و التوثيق الخاص به


* صيانة مستودع [addon-examples](https://github.com/nodejs/node-addon-examples) على الـGitHub، بما في ذلك الشفرة البرمجية، والمشاكل والتوثيق الخاص به
* صيانة اضافة واجهة برمجة التطبيق المكتوبة بالـ C++ الخاصة بمشروع النود جي اس، تحت اشراف لجنة التوجيه التقني.
* صيانة التوثيق الخاص بالإضافة في إطار مشروع النود جي اس تحت اشراف لجنة التوجيه التقني للنود جي اس.
* صيانة حزمة _nan_ على مدير حزم النود، و اصدار النسخ الجديدة منها حسب ما يقتضيه الأمر.
* التراسل حول مستقبل النود جي اس و واجهة NAN لإعطاء المجتمع ملاحظات مسبقة حول التغييرات المستقبلية.

يمكنك الإطلاع على القائمة الحالية للاعضاء هنا:
[README](https://github.com/nodejs/nan#collaborators).

### [القياس](https://github.com/nodejs/benchmarking)

يتمثل الغرض من مجموعة القياس الحصول على توافق حول مجموعة من المعايير المتفق عليها و الخاصة بالأداء التي يمكن استعمالها لأجل:

* تتبع و تشخيص تحسينات الأداء بين إصدارات النود جي اس
* تجنب التراجع في الأداء في النسخ الأحدث

تشمل مسؤولياتها:
* تحديد معيار واحد أو اكثر يمثل الاستخدام الاعتيادي. عادة ما يتطلب ذلك اكثر من معيار واحد لتغطية حالات الاستخدام الاعتيادية للنود جي اس، بما في ذلك الاستخدام الذي يشمل تزامنا عالياً و مكوناً منخفضاً.
* العمل لأجل الحصول على موافقة المجتمع على قائمة المعايير المختارة
* إضافة تنفيذ دوري لجملة من المعايير المختارة لنسخ النود جي اس
* تتبع و نشر نتائج الأداء بين مختلف الاصدارات و النسخ المبنية

### [البناء](https://github.com/nodejs/build)

إن غرض مجموعة البناء هو إنشاء و صيانة بنية تحتية مؤتمتة و موزعة.

تشمل مسؤولياتها:
* إنتاج الحزم لجميع المنصات المستهدفة
* إجراء الاختبارات.
* إجراء اختبارات الأداء و المقارنات.
* إنشاء و تسيير حاويات البناء

### [التشخيص](https://github.com/nodejs/diagnostics)

The Diagnostics Working Group's purpose is to surface a set of comprehensive,
documented, and extensible diagnostic interfaces for use by Node.js tools and
JavaScript VMs.

Responsibilities include:
* Collaborating with V8 to integrate `v8_inspector` into Node.js.
* Collaborating with V8 to integrate `trace_event` into Node.js.
* Collaborating with Core to refine `async_wrap` and `async_hooks`.
* Maintaining and improving OS trace system integration (e.g. ETW, LTTNG, dtrace).
* Documenting diagnostic capabilities and APIs in Node.js and its components.
* Exploring opportunities and gaps, discussing feature requests, and addressing
  conflicts in Node.js diagnostics.
* Fostering an ecosystem of diagnostics tools for Node.js.
* Defining and adding interfaces/APIs in order to allow dumps to be generated
  when needed.
* Defining and adding common structures to the dumps generated in order to
  support tools that want to introspect those dumps.

### [دوكر](https://github.com/nodejs/docker-node)

The Docker Working Group's purpose is to build, maintain, and improve official
Docker images for the Node.js project.

Responsibilities include:
* Keeping the official Docker images updated in line with new Node.js releases.
* Decide and implement image improvements and/or fixes.
* Maintain and improve the images' documentation.

### [الإدماج](https://github.com/nodejs/evangelism)

The Evangelism Working Group promotes the accomplishments
of Node.js and lets the community know how they can get involved.

Responsibilities include:
* Facilitating project messaging.
* Managing official project social media.
* Handling the promotion of speakers for meetups and conferences.
* Handling the promotion of community events.
* Publishing regular update summaries and other promotional
  content.

### i18n

The i18n Working Groups handle more than just translations. They
are endpoints for community members to collaborate with each
other in their language of choice.

Each team is organized around a common spoken language. Each
language community might then produce multiple localizations for
various project resources.

Responsibilities include:
* Translating any Node.js materials they believe are relevant to their
  community.
* Reviewing processes for keeping translations up to date and of high quality.
* Managing and monitoring social media channels in their language.
* Promoting Node.js speakers for meetups and conferences in their language.

Each language community maintains its own membership.

* [nodejs-ar - Arabic (العَرَبِيَّة)](https://github.com/nodejs/nodejs-ar)
* [nodejs-bg - Bulgarian (български)](https://github.com/nodejs/nodejs-bg)
* [nodejs-bn - Bengali (বাংলা)](https://github.com/nodejs/nodejs-bn)
* [nodejs-zh-CN - Chinese (中文)](https://github.com/nodejs/nodejs-zh-CN)
* [nodejs-cs - Czech (Čeština)](https://github.com/nodejs/nodejs-cs)
* [nodejs-da - Danish (Dansk)](https://github.com/nodejs/nodejs-da)
* [nodejs-de - German (Deutsch)](https://github.com/nodejs/nodejs-de)
* [nodejs-el - Greek (Ελληνικά)](https://github.com/nodejs/nodejs-el)
* [nodejs-es - Spanish (Español)](https://github.com/nodejs/nodejs-es)
* [nodejs-fa - Persian (فارسی)](https://github.com/nodejs/nodejs-fa)
* [nodejs-fi - Finnish (Suomi)](https://github.com/nodejs/nodejs-fi)
* [nodejs-fr - French (Français)](https://github.com/nodejs/nodejs-fr)
* [nodejs-he - Hebrew (עברית)](https://github.com/nodejs/nodejs-he)
* [nodejs-hi - Hindi (हिन्दी)](https://github.com/nodejs/nodejs-hi)
* [nodejs-hu - Hungarian (Magyar)](https://github.com/nodejs/nodejs-hu)
* [nodejs-id - Indonesian (Bahasa Indonesia)](https://github.com/nodejs/nodejs-id)
* [nodejs-it - Italian (Italiano)](https://github.com/nodejs/nodejs-it)
* [nodejs-ja - Japanese (日本語)](https://github.com/nodejs/nodejs-ja)
* [nodejs-ka - Georgian (ქართული)](https://github.com/nodejs/nodejs-ka)
* [nodejs-ko - Korean (한국어)](https://github.com/nodejs/nodejs-ko)
* [nodejs-mk - Macedonian (Македонски)](https://github.com/nodejs/nodejs-mk)
* [nodejs-ms - Malay (بهاس ملايو‎)](https://github.com/nodejs/nodejs-ms)
* [nodejs-nl - Dutch (Nederlands)](https://github.com/nodejs/nodejs-nl)
* [nodejs-no - Norwegian (Norsk)](https://github.com/nodejs/nodejs-no)
* [nodejs-pl - Polish (Język Polski)](https://github.com/nodejs/nodejs-pl)
* [nodejs-pt - Portuguese (Português)](https://github.com/nodejs/nodejs-pt)
* [nodejs-ro - Romanian (Română)](https://github.com/nodejs/nodejs-ro)
* [nodejs-ru - Russian (Русский)](https://github.com/nodejs/nodejs-ru)
* [nodejs-sv - Swedish (Svenska)](https://github.com/nodejs/nodejs-sv)
* [nodejs-ta - Tamil (தமிழ்)](https://github.com/nodejs/nodejs-ta)
* [nodejs-tr - Turkish (Türkçe)](https://github.com/nodejs/nodejs-tr)
* [nodejs-zh-TW - Taiwanese (國語)](https://github.com/nodejs/nodejs-zh-TW)
* [nodejs-uk - Ukrainian (Українська)](https://github.com/nodejs/nodejs-uk)
* [nodejs-vi - Vietnamese (Tiếng Việt)](https://github.com/nodejs/nodejs-vi)

### [الإصدارات](https://github.com/nodejs/LTS)
The Release Working Group manages the release process for Node.js.

Responsibilities include:
* Define the release process.
* Define the content of releases.
* Generate and create releases.
* Test Releases.
* Manage the Long Term Support and Current branches including
  backporting changes to these branches.
* Define the policy for what gets backported to release streams

### [الحماية](https://github.com/nodejs/security-wg)

The Security Working Group manages all aspects and processes linked to Node.js security.

Responsibilities include:
* Define and maintain security policies and procedures for:
  * the core Node.js project
  * other projects maintained by the Node.js Technical Steering Committee (TSC).
* Work with the Node Security Platform to bring community vulnerability data into
  the foundation as a shared asset.
* Ensure the vulnerability data is updated in an efficient and timely manner.
  For example, ensuring there are well-documented processes for reporting
  vulnerabilities in community modules.
* Review and recommend processes for handling of security reports (but not the
  actual administration of security reports, which are reviewed by a group of people
  directly delegated to by the TSC).
* Define and maintain policies and procedures for the coordination of security
  concerns within the external Node.js open source ecosystem.
* Offer help to npm package maintainers to fix high-impact security bugs.
* Maintain and make available data on disclosed security vulnerabilities in:
  * the core Node.js project
  * other projects maintained by the Node.js Foundation technical group
  * the external Node.js open source ecosystem
* Promote the improvement of security practices within the Node.js ecosystem.
* Recommend security improvements for the core Node.js project.
* Facilitate and promote the expansion of a healthy security service and product
  provider ecosystem.

### [التدفق](https://github.com/nodejs/readable-stream)

The Streams Working Group is dedicated to the support and improvement of the
Streams API as used in Node.js and the npm ecosystem. We seek to create a
composable API that solves the problem of representing multiple occurrences
of an event over time in a humane, low-overhead fashion. Improvements to the
API will be driven by the needs of the ecosystem; interoperability and
backwards compatibility with other solutions and prior versions are paramount
in importance.

Responsibilities include:
* Addressing stream issues on the Node.js issue tracker.
* Authoring and editing stream documentation within the Node.js project.
* Reviewing changes to stream subclasses within the Node.js project.
* Redirecting changes to streams from the Node.js project to this project.
* Assisting in the implementation of stream providers within Node.js.
* Recommending versions of `readable-stream` to be included in Node.js.
* Messaging about the future of streams to give the community advance notice of changes.

### [الموقع](https://github.com/nodejs/nodejs.org)

The Website Working Group's purpose is to build and maintain a public
website for the Node.js project.

Responsibilities include:
* Developing and maintaining a build and automation system for nodejs.org.
* Ensuring the site is regularly updated with changes made to Node.js, like
  releases and features.
* Fostering and enabling a community of translators.
