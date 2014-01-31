---
layout: post
title: JavaBetiği
---

Aşağıdaki kod:

```javascript
document.write(new Date().toLocaleString()   + '<br/>');
document.write('IİĞÜŞÖÇ'.toLocaleLowerCase() + '<br/>');
document.write('ıiğüşöç'.toLocaleUpperCase() + '<br/>');
```

Wine üzerinde IE6+SP1'de şunu yazıyor:

  24 Nisan 2007 Salı 01:00:28
  iiğüşöç
  IIĞÜŞÖÇ

Doğal ortamında IE6+SP2'de şöyle:

  24 Nisan 2007 Salı 01:00:28
  iİğüşöç
  ıIĞÜŞÖÇ

Firefox (Iceweasel) sürüm 2.0.0.2'de ise şöyle:

  Sal 24 Nis 2007 01:07:08 EEST
  iiğüşöç
  IIĞÜŞÖÇ

Üstad Brian Kernighan'ın Princeton'da verdiği bir ders var: ["Computers in Our
World"](http://www.cs.princeton.edu/courses/archive/fall06/cos109/).  Bu
derste sunulan ["Toy Machine
Simulator"](http://www.cs.princeton.edu/courses/archive/fall06/cos109/toysim.html)
isimli JavaScript'le yazılmış basit benzeticiyi Türkçe'ye uyarlayalım dedik
(sağolsun [Nurettin](http://www.omu.edu.tr/~nurettins) büyük ölçüde halletti).
Fakat `yükle` (`load`), `çıkar` (`subtract`) buyrukları falan derken işlerin
beklenildiği gibi seyretmediğini görünce yukarıdaki denemeleri yapmak
durumunda kaldım.  Bu ECMAScript nanesini yanlış anlamıyorum değil mi?
'`toLowerCase`' var, bir de yerel duyarlı olması umulan '`toLocaleLowerCase`'
var; ne âlâ!  "Yereli doğru mu algılıyor bu?' kaygısıyla `new
Date().toLocaleString()`' şeysini özellikle bastırıyorum, orada sorun yok
gözüküyor.  '`toLocale{Lower,Upper}Case`' işlevlerinin semantiğinden emin
olmak için David Flanagan'ın ["JavaScript The Definitive
Guide"](http://books.google.com/books?q=javascript+the+definitive+guide&oi=print&ct=title)
kitabına da baktım.  Buyurun '`String.toLocaleLowerCase`' maddesini okuyalım
(vurgular bana ait):

> A copy of _string_, converted to lowercase letters in a locale-spesific way.
> Only a few languages, **such as Turkish**, have locale-spesific case
> mappings, so this method usually returns the same value as `toLowerCase()`.

İşin içinde başka bir bit yeniği yoksa şayet (ilgilenenler benimle irtibata
geçebilirler) nurtopu gibi bir Türkçe böceği var ortada, en alışıldık
türden...

Şimdi bu böceği ezseniz n'olur ezmeseniz n'olur (şüphesiz ki raporlanması
gerekiyor; en azından Mozilla Seamonkey grubuna)?  JavaScript hataları
["erörlü pul"](http://www.eksisozluk.com/show.asp?t=eror) gibidir;
kıymetlidir, bir kere ortaya çıkmışsa sittin sene (sittin: Arapça'da 60)
başbaşa yaşarsınız! :-) (Daha iyisini buluncaya kadar) aşağıdaki gibi bir
geçici çözüm uydurdum buna.  Google'dan böyle bir şey aratan olursa ağa
takılsın diye buraya koyuyorum:

```javascript
var TURKISH_CODEPOINTS = '\u00e2\u00e7\u011f\u0131\u00f6\u00fb\u00fc\u015f' +
                         '\u00c2\u00c7\u011e\u0130\u00d6\u00db\u00dc\u015e';
var TURKISH_LETTERS    = 'a-zA-Z' + TURKISH_CODEPOINTS;

var _turkish_hack = { lower:[], upper:[] };
if ('I'.toLocaleLowerCase != '\u0131')
        _turkish_hack.lower.push({ se:new RegExp('I', 'g'),       re:'\u0131' });
if ('\u0130'.toLocaleLowerCase != 'i')
        _turkish_hack.lower.push({ se:new RegExp('\\u0130', 'g'), re:'i'      });
if ('\u0131'.toLocaleUpperCase != 'I')
        _turkish_hack.upper.push({ se:new RegExp('\\u0131', 'g'), re:'I'      });
if ('i'.toLocaleUpperCase != '\u0130')
        _turkish_hack.upper.push({ se:new RegExp('i', 'g'),       re:'\u0130' });

if (_turkish_hack.lower.length)
        String.prototype.toTurkishLowerCase = function () {
                var fixed = this;
                for (var i in _turkish_hack.lower)
                        fixed = fixed.replace(
                                _turkish_hack.lower[i].se,
                                _turkish_hack.lower[i].re
                        );
                return fixed.toLocaleLowerCase();
        }
else String.prototype.toTurkishLowerCase = String.prototype.toLocaleLowerCase;

if (_turkish_hack.upper.length)
        String.prototype.toTurkishUpperCase = function () {
                var fixed = this;
                for (var i in _turkish_hack.upper)
                        fixed = fixed.replace(
                                _turkish_hack.upper[i].se,
                                _turkish_hack.upper[i].re
                        );
                return fixed.toLocaleUpperCase();
        }
else String.prototype.toTurkishUpperCase = String.prototype.toLocaleUpperCase;
```

Problem '`ıİ`' harflerinde olduğundan sadece onlar için kendinden ayarlı bir
şeyler kodladım.  '`toLocale{Lower,Upper}Case`' yerine
'`toTurkish{Lower,Upper}Case`' kullanılması yeterli olmalı (fazla deneme
yapmadım, hataları olabilir).  Bir de '`TURKISH_LETTERS`' adında bir _karakter
aralığı_ tanımladım.  Düzenli ifadelerde faydalı olabilir.  Şunun gibi
('`toysim.js`'den alıntı):

```javascript
op = s.replace(new RegExp('^[_' + TURKISH_LETTERS + ']\\S*'), "") // zap label if present
```
