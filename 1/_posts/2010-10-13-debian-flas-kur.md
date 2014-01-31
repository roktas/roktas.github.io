---
layout: post
title: Hibrit ISO ile Flaş'tan Debian Kurulumu
---

Aşağıdaki yöntemle önyüklemeyi flaş'tan, kurulumu ağ'dan yapmak mümkün.
**Dikkat!**   Bu yöntemde İnternet erişimine sahip bir ağ bağlantınız olmalı.

1. 16 MB'lık `mini.iso`'yu indir.

        $ wget http://people.debian.org/~joeyh/d-i/images/daily/netboot/mini.iso

2. Flaş'a kaydet.  Örneğin flaş için `/dev/sdb` aygıtı atanmışsa:

        $ cat mini.iso >/dev/sdb

   **Dikkat!**  `/dev/sdb` gibi bir aygıt ismi kullanacaksınız, `/dev/sdb1`
   gibi bir bölüm ismi **değil**.  Flaş için hangi aygıtın atandığını çabucak
   öğrenmek için `mount` veya `fdisk -l` komutlarını kullanabilirsiniz.

3. Flaş'ı çıkarıp tekrar tak.  Bağlama noktasındaki `Firmware` dizinine
   (çoğunlukla `/media/Firmware`) [aygıt yazılımları
   paketini](http://cdimage.debian.org/cdimage/unofficial/non-free/firmware/testing/current/firmware.tar.gz)
   kopyala.
   
4. Flaş'ı (güvenli şekilde) çıkar ve kurulum yapılacak makineyi flaş'la aç.

5. Önyüklemeden sonra kurulumu ağdan tamamla.

Bu yöntemi, PXE boot ile önyükleme de dahil, tamamen ağdan gerçekleşen
kurulumdan farklı veya pratik kılan ne?  Ağdan önyükleme için bir sunucu
hazırlamanız gerekmiyor.  Peki CD'den farkı?  Sadece 16 MB'lık bir imaj
yeterli oluyor.

Ağ'ın çalışır durumda olması önemli.  Standart bir ethernet bağdaştırıcı ile
gerçekleşen bir kablolu bağlantınız varsa sorun olmaz, ama örneğin makinede
sadece kablosuz erişim varsa kablosuz aygıt yazılımı (firmware) önyükleme
sırasında hazır olmalı.

Kaynak: [Debian USB install from hybrid iso -- Joey Hess](http://kitenet.net/~joey/blog/entry/Debian_USB_install_from_hybrid_iso/)
