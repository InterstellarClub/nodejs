---
title: ECMAScript 2015 (ES6) و ما بعدها
layout: docs.hbs
---
# ECMAScript 2015 (ES6) و ما بعدها

تم بناء الـ Node.js باستعمال نسخ حديثة من الـ [V8](https://developers.google.com/v8/)، و هذا يضمن اتاحة آخر المميزات الخاصة بالجافاسكريبت و الموافقة لـ [مواصفات JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm) للمطورين في الوقت المناسب، إضافة إلى التحسينات المستمرة في الأداء و الثبات.

تقسم مميزات الـ ECMAScript 2015 (ES6) إلى ثلاثة مجموعات: **المميزات التي تم شحنها** و **المميزات التي سيتم شحنها** و **المميزات قيد التقدم** حيث:

* أن كافة **المميزات التي تم شحنها** ، و التي يعتبرها الـ V8 ثابتة **يتم تشغيلها تلقائيا على الـ Node.js** و لا تتطلب أي نوع من الاعلام في وقت التشغيل.
* أن **المميزات التي سيتم شحنها** و التي هي مميزات مكتملة تقريبا و لكنها لا تعتبر ثابتة حسب فريق تطوير الـ V8 تتطلب علما في وقت التشغيل لاستعمالها: `--harmony`
* أن **المميزات قيد التقدم** يمكن تشغيلها فرديا عبر العلم الخاص بها، رغم أن هذا الأمر منصوح بشدة تجنبه إلا لاغراض الاختبار. ملاحظة: هذه الأعلام معرفة من قبل الـ V8 و من الممكن لها ان تتغير دون إشعار بذلك.

## أي من المميزات تشحن مع أي نسخة من الـ Node.js إفتراضيا ؟

يوفر موقع [node.green](http://node.green) نظرة عامة ممتازة حول مميزات الـ ECMAScript المدعومة في مختلف نسخ الـ Node.js بناء على جدول كانغاكس.

## أي من المميزات هي قيد التقدم ؟


New features are constantly being added to the V8 engine. Generally speaking, expect them to land on a future Node.js release, although timing is unknown.

You may list all the *in progress* features available on each Node.js release by grepping through the `--v8-options` argument. Please note that these are incomplete and possibly broken features of V8, so use them at your own risk:

```bash
node --v8-options | grep "in progress"
```

## What about the performance of a particular feature?

The V8 team is constantly working to improve the performance of new language features to eventually reach parity with their transpiled or native counterparts in EcmaScript 5 and earlier. The current progress there is tracked on the website [six-speed](https://fhinkel.github.io/six-speed), which shows the performance of ES2015 and ESNext features compared to their native ES5 counterparts.

The work on optimizing features introduced with ES2015 and beyond is coordinated via a [performance plan](https://docs.google.com/document/d/1EA9EbfnydAmmU_lM8R_uEMQ-U_v4l9zulePSBkeYWmY), where the V8 team gathers and coordinates areas that need improvement, and design documents to tackle those problems.

## I have my infrastructure set up to leverage the --harmony flag. Should I remove it?

The current behaviour of the `--harmony` flag on Node.js is to enable **staged** features only. After all, it is now a synonym of `--es_staging`. As mentioned above, these are completed features that have not been considered stable yet. If you want to play safe, especially on production environments, consider removing this runtime flag until it ships by default on V8 and, consequently, on Node.js. If you keep this enabled, you should be prepared for further Node.js upgrades to break your code if V8 changes their semantics to more closely follow the standard.

## How do I find which version of V8 ships with a particular version of Node.js?

Node.js provides a simple way to list all dependencies and respective versions that ship with a specific binary through the `process` global object. In case of the V8 engine, type the following in your terminal to retrieve its version:

```bash
node -p process.versions.v8
```
