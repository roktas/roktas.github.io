---
layout: post
title: Sakin Pencere
---

Yeni tanıştığım bir pencere yöneticisi: `calmwm` veya kısaca
[`cwm`](http://www.openbsd.org/cgi-bin/cvsweb/xenocara/app/cwm/).  OpenBSD
taban sistemiyle birlikte gelen ve ilk olarak 4.2 sürümünün [değişiklik
listesinde](http://www.openbsd.org/plus42.html) gözüme çarpan program `fvwm`
ve `wm2`'den sonra şu an OpenBSD'nin öntanımlı X pencere yöneticisi konumunda.

Hafif ve basit programları seviyorum, hele bir de OpenBSD geliştiricilerinin
elinden çıkmışsa.  BSD kitaplıklarını kullanan programın GNU/Linux altında
derlenememek gibi bir sorunu var.  Hafta sonu oturup çabuk bir "taşıma"
çalışması yaptım[1].  Sonuç: programın CVS deposundan ihraç edilen [git
deposu]( http://github.com/roktas/debcwm.git) ve yakında resmi depoya
yükleyeceğim `cwm` [Debian paketi](http://people.debian.org/~roktas/packages/)
(sayfa sonundaki APT talimatlarını izleyin).

Programı Debian (ve türevleri) dışında kurmak isteyeceklere not.  Kaynağı git
deposundan indirdikten sonra derleme sırasında gerekli geliştirme kitaplıkları
için paket bağımlılıklarına veya aşağıdaki çıktıya başvurabilirsiniz:

```bash
$ ldd /usr/bin/cwm
linux-gate.so.1 =>  (0xffffe000)
libXft.so.2 => /usr/lib/libXft.so.2 (0xb7fad000)
libX11.so.6 => /usr/lib/libX11.so.6 (0xb7ec1000)
libXext.so.6 => /usr/lib/libXext.so.6 (0xb7eb2000)
libfontconfig.so.1 => /usr/lib/libfontconfig.so.1 (0xb7e88000)
libXinerama.so.1 => /usr/lib/libXinerama.so.1 (0xb7e85000)
libXrandr.so.2 => /usr/lib/libXrandr.so.2 (0xb7e7f000)
libbsd.so.0 => /lib/libbsd.so.0 (0xb7e78000)
libc.so.6 => /lib/i686/cmov/libc.so.6 (0xb7d2b000)
libfreetype.so.6 => /usr/lib/libfreetype.so.6 (0xb7cbb000)
libz.so.1 => /usr/lib/libz.so.1 (0xb7ca6000)
libXrender.so.1 => /usr/lib/libXrender.so.1 (0xb7c9d000)
libXau.so.6 => /usr/lib/libXau.so.6 (0xb7c9a000)
libXdmcp.so.6 => /usr/lib/libXdmcp.so.6 (0xb7c95000)
libdl.so.2 => /lib/i686/cmov/libdl.so.2 (0xb7c91000)
libexpat.so.1 => /usr/lib/libexpat.so.1 (0xb7c6a000)
/lib/ld-linux.so.2 (0xb7fe0000)
libpthread.so.0 => /lib/i686/cmov/libpthread.so.0 (0xb7c52000)
```

Bu çıktıda X kitaplıklarının yanısıra bir kitaplık gözünüze çarpmış olabilir:
`libbsd`.  BSD sistemlerinde sıkça kullanılan işlevleri içeren kitaplığı
[buradan](http://libbsd.freedesktop.org/wiki/) indirebilirsiniz (kitaplık
içinde sadece `strtonum` yoktu, onu kaynak kod ağacına ekledim).

Bu tür "yöneten" programları X altında denemek biraz sıkıntılı oluyor,
sisteminizin o sıradaki etkin yöneticisinden dolayı.  Git deposunda
[Xephyr](http://freedesktop.org/wiki/Software/Xephyr) kullanan bir
`debian/try-wm.sh` isimli bir betik bulacaksınız, mevcut X oturumunuzu
sonlandırmadan `cwm`'i deneyebilirsiniz.

[1] Bu konuda bir [başkasının](http://www.martintoft.dk/?p=cwm) çalışmasından
da bahsedelim.  Başlangıç olarak yararlı olmakla birlikte, bu arkadaşın
yazdığı `Makefile`'da fazla tekrar vardı ve benim yaptığım gibi `libbsd`
kitaplığını kullanmamıştı.
