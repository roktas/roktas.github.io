---
layout: post
title: Debian-installer Hakkında Karalamalar
---

Debian-installer rüştünü ispatlamış Debian tekniklerinin etkin şekilde
kullanıldığı bir proje.  Nedir bu teknikler?

- Kurulum modüllerinin birer mikro Debian paketi olması (`udeb`)

  Aslında hikayenin başladığı yer burası.  Herşey bir paket olunca aşağıda
  sıralanan olumluluklar kendiliğinden geliyor.  Mikro Debian paketlerini
  normal paketlerden ayıran husus, paket boyutunu küçültecek her türlü imkanın
  bu paketler üzerinde uygulanması.  Normal paketler için geçerli "Debian
  şartnamesi" (Policy) bu paketler için geçerli değil mesela.

- Hata takibi için BTS'in (Debian Hata Takip Sistemi) kullanılması

  Kanaatimce BTS'i önemli yapan husus Debian iş akışının ana dilini oluşturan
  bir "jargon" sunması.  Bu jargon bir kere benimsendiğinde öylesine zengin
  olanaklar sunuyor ki bazen çok farklı bir dil konuşuyormuş izlenimine de
  kapılıyorsunuz.  Geliştirici cephesinden bakıldığında BTS'in kullanımı çok
  kolay ve galiba gücünü de basitliğinden alıyor.

  Sadece debian-installer hatalarını takip etmeye yönelik olarak eklenen bir
  '`d-i`' etiketi ve kurulum raporları için kurgulanmış bir
  '`installation-reports`' sanal paketiyle BTS'in projeyle entegrasyonu etkin
  şekilde gerçekleştiriyor.

- Debconf altyapısının kullanılması

  Debconf ESR'ın ayrıntılı şekilde belgelediği [Un*x
  Zen](http://www.catb.org/~esr/writings/taoup/html/)'iyle uyumlu bir
  yapılandırma sistemi.  Paket yapılandırma betikleri Debconf protokolü
  denilen, SMTP'ye benzer tek satırlık komutlarla Debconf ile haberleşiyor.
  Bu haberleşme sonucunda kullanıcıyla etkileşime giren önyüz pakete ilişkin
  ayarları kullanıcıdan alıyor ve paketi yapılandırıyor.

  İlk tasarımı takip eden dönemde Perl tabanlı Debconf kitaplığı yazılmıştı.
  Debconf'un d-i'a entegrasyonunda ilk adım bu kitaplığın C ile baştan
  yazılmasıydı.  Bu çalışmaların sonucunda '`cdebconf`' doğdu.  D-i'ın
  iskeletini oluşturan birkaç modülden birisi de budur.  Debian-installer esas
  itibarıyla alt yapıda Debconf'un kullanıldığı, merkezinde kurulum
  modüllerini çalıştıran ana menü (`main-menu`) modülünün bulunduğu bir
  modüller bütünüdür.

  D-i bugün itibarıyla '`newt`' kitaplığına dayalı metin tabanlı bir GUI olan
  '`dialog`' önyüzünü kullanıyor.  Son kullanıcı veya _eye candy_ düşkünleri
  için GNOME tabanlı bir önyüz kullanılması da planlanıyor.  Bu
  gerçekleştiğinde sanıyorum şuna benzer bir şeyler çıkacaktır ortaya:

  ![Grub ana yapılandırma ekranı](/images/di-grub-main.png)
  
  ![Grub yardım ekranı](/images/di-grub-help.png)

- Po-debconf

  Debconf'un ilk tasarımında i18n desteğinin dikkate alınmadığını
  söyleyemeyiz.  Fakat bu destek 'GNU gettext' tabanlı değildi.  Neyseki
  yerelleştirme konularında yüksek hassasiyete sahip bir kısım Fransız
  geliştirici sayesinde[^1] bu özellik 'po-debconf' adıyla sisteme eklendi.
  Po-debconf'un Debian yerelleştirme çalışmalarında patlama oluşturduğunu
  söylemek abartı olmaz.  Debian-installer şu satırların yazıldığı tarih
  itibarıyla 40 dilde konuşabiliyorsa bunun birinci nedeni po-debconf'dur.
  (İkinci nedeni de Christian Perrier :-)

- Projenin sürülebilirlik kararı için Debian arşiv kurallarının uygulanması

  Birer Debian paketi olduğunu söylediğimiz modüller sürüldüğünde `unstable`'a
  giriyor ve Debian arşiv kuralları çerçevesinde `testing`'e ilerliyor.  Bunun
  sonucunda 'testing' havuzundan inşa edilen günlük d-i CD'leri ortaya
  çıkıyor.  Yani 'testing'de (ki bir sonraki kararlı sürüme taban teşkil
  edeceğinden bu arşivin diğer adı `sarge`) toplanan paketler bir dizi kriteri
  de sağlamış olduğundan günlük d-i CD'lerinin göreceli olarak kararlı
  olduğunu söyleyebiliyoruz.
  
- Çoklu platform

  Debian'ın 11 farklı platformda çalışmasının arkasında '`buildd`' dediğimiz
  otomatik inşa betikleri yatıyor.  Aynı betikler d-i paketlerini de
  işlediğinden kurulum programını `i386` platformu dışında da
  çalıştırabiliyoruz.  Modüllerin bir platformda derlenmesi d-i'ın o
  platformda çalışması için yeterli değil tabii.  (Özellikle `sparc` ve `s390`
  RC1'e kadar çok sorun çıkarıyordu.)

- APT entegrasyonu:

  "_Anna's not nearly apt_".  D-i'ın çok sayıda kurulum senaryosunu
  desteklemesinin arkasında '`anna`' modülü yatıyor.  Bu modül sayesinde
  udeb'ler bir dizi 'alıcı' betikle çeşitli kaynaklardan alınarak APT tarzı
  kurulabiliyor.

- Debootstrap entegrasyonu:
  
  'Boot-floppies' günlerinden kalan bu klasik Debian aracıyla bir hedef kafes
  ortamına Debian iskeleti kurulabiliyor.  Debootstrap'ın d-i'a entegrasyonu,
  olgunluğunu ispatlamış Debian mirasının yeni birşeyler keşfetmeye lüzum
  bırakmadan kullanılabileceğine güzel bir örnek.

^[1] Dillerini korumak için yasa çıkaran, Cemil Meriç'in ifadesiyle Dünya
Edebiyatının sekreteryalığını yapan bir ulus olarak Fransızların i18n
duyarlılıklarını anlamak mümkün.  GNU gettext'in şef geliştiricisi Francois
Pinard'ı anmak isterim hemen.  Debian kapsamında
[po](http://www.debian.org/international/l10n/po/rank) ve
[po-debconf](http://www.debian.org/international/l10n/po-debconf/rank)
çevirilerinde eriştikleri yüksek yüzdeyi de belirtmeden geçemeyeceğim.
(po-debconf çevirilerinde 50'ye yakın dil arasında Türkçe 10'uncu geliyor.
Büyük ölçüde d-i çevirileriyle yakalanan ve tabii daha da yukarılara çekmemiz
gereken bir oran bu.)

[archived]: http://bugs.debian.org/cgi-bin/pkgreport.cgi?which=submitter&amp;data=roktas%40omu.edu.tr&amp;archive=yes
[active]: http://bugs.debian.org/cgi-bin/pkgreport.cgi?which=submitter&amp;data=roktas%40omu.edu.tr&amp;archive=no
