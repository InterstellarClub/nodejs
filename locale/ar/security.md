---
layout: security.hbs
title: Security
---

# الأمن

## الإبلاغ عن ثغرة في الـ Node.js

قم بالتبليغ بأي ثغرة في الـ Node.js عبر منصة [HackerOne](https://hackerone.com/nodejs) أو عبر مراسلة البريد الالكتروني [security@nodejs.org](mailto:security@nodejs.org).

سيتم مراجعة تقريرك في غضون 24 ساعة، و ستتلقى إجابة مفصلة عن تقريرك في غضون 48 ساعة، حيث سترشدك هذه الإجابة إلى الخطوة التالية فيما يخص المعلومات التي أدليت بها.

بعد تلقيك الإجابة المبدئية عن تقريرك، سيعلمك الفريق الأمني حول التطورات التي تحل فيما يخص إصلاح و إعلان الثغرة، كما قد يُطلب منك معلومات إضافية أو إرشادات حول المشكلة المُبلغ عنها.
ترسل هذه التحديثات من الفريق الأمني كل خمسة أيام على الأقل، و في الواقع، فإن هذا  ما يحدث عادة كل 24 إلى 48 ساعة.
### برنامج المكافآت الخاص بالـ Node.js

لدى الـ Node.js برنامج رسمي للمكافآت الأمنية، للباحثين الأمنيين و مكتشفي الثغرات عموما.

تتم إدارة هذا البرنامج عبر منصة HackerOne على [https://hackerone.com/nodejs](https://hackerone.com/nodejs) لتفاصيل أكثر.

## Reporting a Bug in a third party module

Security bugs in third party modules should be reported to their respective maintainers and should also be coordinated
through the [Node Ecosystem Security Team](https://hackerone.com/nodejs-ecosystem) or by emailing 
[security-ecosystem@nodejs.org](mailto:security-ecosystem@nodejs.org).

Details regarding this process can be found in the [Security Working Group repository](https://github.com/nodejs/security-wg/blob/master/processes/third_party_vuln_process.md).

Thank you for improving the security of Node.js and its ecosystem. Your efforts and responsible disclosure are greatly
appreciated and will be acknowledged.

## Disclosure Policy

Here is the security disclosure policy for Node.js

- The security report is received and is assigned a primary handler. This person will coordinate the fix and release
process. The problem is confirmed and a list of all affected versions is determined. Code is audited to find any
potential similar problems. Fixes are prepared for all releases which are still under maintenance. These fixes are not
committed to the public repository but rather held locally pending the announcement.

- A suggested embargo date for this vulnerability is chosen and a CVE (Common Vulnerabilities and Exposures (CVE®))
is requested for the vulnerability.

- On the embargo date, the Node.js security mailing list is sent a copy of the announcement. The changes are pushed to
the public repository and new builds are deployed to nodejs.org. Within 6 hours of the mailing list being notified, a
copy of the advisory will be published on the Node.js blog.

- Typically the embargo date will be set 72 hours from the time the CVE is issued. However, this may vary depending on
the severity of the bug or difficulty in applying a fix.

- This process can take some time, especially when coordination is required with maintainers of other projects. Every
effort will be made to handle the bug in as timely a manner as possible; however, it’s important that we follow the
release process above to ensure that the disclosure is handled in a consistent manner.


## Receiving Security Updates

Security notifications will be distributed via the following methods.

- [https://groups.google.com/group/nodejs-sec](https://groups.google.com/group/nodejs-sec)
- [https://nodejs.org/en/blog](https://nodejs.org/en/blog)

## Comments on this Policy

If you have suggestions on how this process could be improved please submit a [pull request](https://github.com/nodejs/nodejs.org)
or [file an issue](https://github.com/nodejs/security-wg/issues/new) to discuss.
