---
layout: post
title: Bir Çekimin Çekimi
---

Şöyle bir şey:

![19/x uçbirim çekimi](/images/x-screen-cast.gif)

Burada ne dönüyor?  Uçbirim çekimlerinin standart bir stüdyo ortamında nasıl
gerçekleştiğini anlatıyoruz.  ÖYAK florasından seçtiğimiz bir kaç aracı bir
araya getirerek: `byzanz`, `tilda`, `xdotool`, `key-mon` ve tabii `19/x`.

- GNOME panelinde görülen "19/x" ikonu 800x600 geometriye sahip dekorasyonsuz
  bir uçbirim açarak 19/x'i `screencast` profilinde çalıştırıyor.  Uçbirimin
  üzerine de `key-mon` marifetiyle çekim sırasında yapılan tüm tuş vuruşlarını
  gösteren bir tür "overlay" bindiriyoruz.  Bu ilk aşama, yani belli
  özelliklerde (buna "standart" dedik) bir uçbirim stüdyosu kuruyoruz öncelikle.

- Tek bir kliklemeyle hazır hale getirdiğimiz stüdyoda çekime başlıyoruz.
  Nasıl?  GNOME panelinde görülen kırmızı ikona tıklayarak `byzanz`'ı
  çalıştırıyoruz.  Dosya ismini `.gif` uzantılı olarak verdikten sonra çekimin
  yapılacağı pencereyi seçiyoruz, ilk aşamada açılan uçbirimde herhangi bir yere
  tıklayarak...

Bu şekilde oluşan (örnekte `test.gif`) GIF canlandırmayı herhangi bir tarayıcıda
açarak izleyebilirsiniz.  Örnekte görüldüğü gibi tuş vuruşları da otomatik
olarak kaydediliyor, Vim tutoryalleri için birebir.  Ama buradaki (şu yazıyı
yazdıran) asıl numara "standart" çekim ortamının tek tıklamayla hazır hale
getirilmesi.  Bu fakir tarafından kodlanan şöyle bir şey sayesinde:
[`x-screen-cast`](https://github.com/00010011/x/blob/master/bin/x-screen-cast)...

GIF canlandırmaların izlerken durdurulamaması bir sorun, fakat yöntemin
basitliği ve her tarayıcıda (herhangi özel bir araç gerekmeden) çalışabilmesi
(çekimin fazla uzun olmaması şartıyla) bu olumsuzluğu unutturuyor.  GIF
canlandırmayı arada durdurarak seyretmek isteyenler `mplayer` kullanabilir.

Şimdi gidip biraz "Karpuzcu Ninja" (Fruit Ninja) oynamalıyım...
