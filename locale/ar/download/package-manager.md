---
layout: page.hbs
title: Installing Node.js via package manager
---

# تثبيت النود جي اس عبر مدير حزم

***ملاحظة*** إن صيانة و دعم الحزم المذكورة في هذه الصفحة تتم عبر المشرفين على مديري الحزم، و **ليس** فريق النود جي اس الأساسي. تفضل بإبلاغ أية مشكلة إلى المشرفين على الحزم و إذا كانت مشكلتك عبارة عن خطأ في النود جي اس بحد ذاتها فسيبلغ المشرف عن هذه المشكلة صعودا.  

----------------------------

* [آندرويد](#android)
* [Arch Linux](#arch-linux)
* [التوزيعات المبنية على ديبيان أو اوبنتو، لينكس للمؤسسات / فيدورا و حزم سناب](#debian-and-ubuntu-based-linux-distributions-enterprise-linux-fedora-and-snap-packages)
* [FreeBSD](#freebsd)
* [Gentoo](#gentoo)
* [NetBSD](#netbsd)
* [nvm](#nvm)
* [OpenBSD](#openbsd)
* [openSUSE and SLE](#opensuse-and-sle)
* [macOS](#macos)
* [SmartOS and illumos](#smartos-and-illumos)
* [Solus](#solus)
* [Void Linux](#void-linux)
* [ويندوز](#windows)

----------------------------

## آندرويد

لا يزال دعم النود جي اس على الاندرويد قيد التجربة، لذلك فإن الملفات الثنائية المنتجة قبلا لا تزال غير متوفرة من قبل مطوري النود جي اس.

رغم ذلك، هناك بعض الحلول الموفرة من طرف ثالث، فمثلا يوفر مجتمع [Termux](https://termux.com/) محاكي طرفية و بيئة لينكس للأندرويد، إضافة إلى مدير حزم خاص و [مجموعة واسعة](https://github.com/termux/termux-packages) من العديد من التطبيقات المنتجة قبلا.
الأمر التالي سيثبت آخر نسخة متوفرة من النود جي اس:

```bash
pkg install nodejs
```
حاليا، النسخ الثنائية الخاصة بـ Termux تنتج بدون الحاجة إلى دعم مراقب و هي مربوطة بـ `system-icu` (تعتمد على حزمة `libicu`).

## Arch Linux

تتوفر حزم النود جي اس و الـ npm على مستوى مستودعات المجتمع.

```bash
pacman -S nodejs npm
```

## التوزيعات المبنية على ديبيان أو اوبنتو، لينكس للمؤسسات / فيدورا و حزم سناب

يتم توفير [الملف الثنائي الرسمي للنود جي اس](https://github.com/nodesource/distributions/blob/master/README.md) من قبل NodeSource.

## FreeBSD

آخر إصدارات النود جي اس متوفرة عبر [www/node](http://freshports.org/www/node)

يمكنك تثبيت حزمة ثنائية عبر [pkg](https://www.freebsd.org/cgi/man.cgi?pkg):

```bash
pkg install node
```
او يمكنك انتاجها باستعمال الـ[ports](https://www.freebsd.org/cgi/man.cgi?ports) الخاص بك:

```bash
cd /usr/ports/www/node && make install
```

## Gentoo

Node.js is available in the portage tree.

```bash
emerge nodejs
```

## NetBSD

Node.js is available in the pkgsrc tree:

```bash
cd /usr/pkgsrc/lang/nodejs && make install
```

Or install a binary package (if available for your platform) using pkgin:

```bash
pkgin -y install nodejs
```

## nvm
Node Version Manager is a bash script used to manage multiple released Node.js versions. It allows
you to perform operations like install, uninstall, switch version, etc.
To install nvm, use this [install script](https://github.com/creationix/nvm#install-script).

On Unix / OS X systems Node.js built from source can be installed using
[nvm](https://github.com/creationix/nvm) by installing into the location that nvm expects:

```bash
$ env VERSION=`python tools/getnodeversion.py` make install DESTDIR=`nvm_version_path v$VERSION` PREFIX=""
```

After this you can use `nvm` to switch between released versions and versions
built from source.
For example, if the version of Node.js is v8.0.0-pre:

```bash
$ nvm use 8
```

Once the official release is out you will want to uninstall the version built
from source:

```bash
$ nvm uninstall 8
```

## OpenBSD

Node.js is available through the ports system.

```bash
/usr/ports/lang/node
```

Using [pkg_add](http://man.openbsd.org/OpenBSD-current/man1/pkg_add.1) on OpenBSD:

```bash
pkg_add node
```

## openSUSE and SLE

Node.js is available in the main repositories under the following packages:

* **openSUSE Leap 42.2**: `nodejs4`
* **openSUSE Leap 42.3**: `nodejs4`, `nodejs6`
* **openSUSE Tumbleweed**: `nodejs4`, `nodejs6`, `nodejs8`
* **SUSE Linux Enterprise Server (SLES) 12**: `nodejs4`, `nodejs6`
  (The "Web and Scripting Module" must be [added before installing](https://www.suse.com/documentation/sles-12/book_sle_deployment/data/sec_add-ons_extensions.html).)

For example, to install Node.js 4.x on openSUSE Leap 42.2, run the following as root:

```bash
zypper install nodejs4
```

## macOS

Simply download the [macOS Installer](https://nodejs.org/#download) direct from the [nodejs.org](https://nodejs.org) web site.

_If you want to download the package with bash:_

```bash
curl "https://nodejs.org/dist/latest/node-${VERSION:-$(wget -qO- https://nodejs.org/dist/latest/ | sed -nE 's|.*>node-(.*)\.pkg</a>.*|\1|p')}.pkg" > "$HOME/Downloads/node-latest.pkg" && sudo installer -store -pkg "$HOME/Downloads/node-latest.pkg" -target "/"
```

### Alternatives

Using **[Homebrew](http://brew.sh/)**:

```bash
brew install node
```

Using **[MacPorts](http://www.macports.org/)**:

```bash
port install nodejs<major version>

# Example
port install nodejs7
```

Using **[pkgsrc](https://pkgsrc.joyent.com/install-on-osx/)**:

Install the binary package:

```bash
pkgin -y install nodejs
```

Or build manually from pkgsrc:

```bash
cd pkgsrc/lang/nodejs && bmake install
```

## SmartOS and illumos

SmartOS images come with pkgsrc pre-installed.  On other illumos distributions, first install **[pkgsrc](https://pkgsrc.joyent.com/install-on-illumos/)**, then you may install the binary package as normal:

```bash
pkgin -y install nodejs
```

Or build manually from pkgsrc:

```bash
cd pkgsrc/lang/nodejs && bmake install
```


## Solus

Solus provides node.js in its main repository.

```bash
sudo eopkg install nodejs
```


## Void Linux

Void Linux ships node.js stable in the main repository.

```bash
xbps-install -Sy nodejs
```

## Windows

Simply download the [Windows Installer](https://nodejs.org/#download) directly from the [nodejs.org](https://nodejs.org) web site.

### Alternatives

Using **[Chocolatey](http://chocolatey.org)**:

```bash
cinst nodejs
# or for full install with npm
cinst nodejs.install
```

Using **[Scoop](http://scoop.sh/)**:

```bash
scoop install nodejs
```
