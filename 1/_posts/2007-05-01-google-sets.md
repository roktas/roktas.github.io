---
layout: post
title: Google Sets
---

[Google Kümeler](http://labs.google.com/sets): verilen örneklerle (Google
aramaları bağlamında) ilişkili ögeleri içeren kümeyi, isteğe bağlı olarak kısa
veya uzun şekilde, listeleyen bir Google servisi.  Komut satırından çabucak
denemek için bir betik yazdım: [`gibi`](http://gist.github.com/410669).

```bash
$ ./gibi "cüneyt arkın" "kadir inanır"
Querying for the terms: cüneyt arkın kadir inanır...

kadir inanır
cüneyt arkın
aytekin akkaya
kemal sunal
türkan şoray
hüseyin peyda
mehmet ali erbil
füsun uçar
kartal tibet
kadran
fatma girik
kenan pars
korhan abay
kenan çoban
kenan imirzalıoğlu
```

Listede tanımadığım isimler de var, ama sonuç fena değil.  Daha uzun bir liste
de üretilebilir, betiği farklı bir isimde (`gibigibi`) çalıştırarak:

```bash
$ ln -s gibi gibigibi
$ ./gibigibi | wc -l
Querying for the terms: cüneyt arkın kadir inanır...

47
```

Böyle bir servisin çeşitli kullanımları olabilir.  Mesela bir türlü
hatırlayamadığınız bir isime ilgili alanda aklınıza gelen birkaç kelimeyi
kullanarak erişebilirsiniz.  "Buluş anları"nda zihni kışkırtıcı bir araç
olarak da başvurulabilir.
