---
layout: post
title: Sıralı İsimlendirilmiş Dosyaların İndirilmesi
---

Bir web sitesinde sıralı şekilde isimlendirilmiş dosyalar bulunuyor.  Örneğin:

```
http://foo.bar/files/article/1.pdf
http://foo.bar/files/article/2.pdf
...
http://foo.bar/files/article/130.pdf
```

Bu dosyaları komut satırından nasıl indiririm?

---

[Emin](http://emineker.net)'den gelen cevap:

```bash
$ curl -O http://foo.bar/files/article/[1-120].pdf
```

[Emre](http://ecylmz.com)'den gelen cevap:

```bash
$ for i in {1..120}; do wget http://foo.bar/files/article/$f.pdf; done
```


Yalnız bakın burada şuna dikkat edin.  Joker karakterleri açan shell'dir.
Örneğin `ls *.pdf` dediğinizde shell bulunduğunuz dizinde pdf uzantılı dosyalar
_varsa_, "`*.pdf`"yi bir listeye çevirir ve ls'e argüman olarak verir.  Yoksa?
"`*.pdf`"'yi olduğu gibi verir.  Örnek:

```bash
$ touch olmayan_dosya.1 olmayan_dosya.2
$ ls *.[1-2] # [1] notuna bakın
olmayan_dosya.1 olmayan_dosya.2
$ rm -f *.[1-2]
$ ls *.[1-2]
ls: *.[1-2]'e erişilemedi: Böyle bir dosya ya da dizin yok
```

Kabuğun joker karakter açma semantiği böyle.  Şimdi buna bakalım:

```bash
$ curl -O http://foo.bar/files/article/[1-120].pdf
```

`[1-120].pdf` jokerini shell açamaz, her şey bir yana "http://" vesaireden
dolayı.  Yani bunu olduğu gibi curl'e ver.  Demek ki burada `[1-120].pdf`
ifadesinden `1.pdf`, `2.pdf`, .. `120.pdf` ardışımını üreten kod curl içinde
kabukta değil.  Bu inceliği not ederseniz kabukla ilgili önemli bir noktayı
kavramış olursunuz.
