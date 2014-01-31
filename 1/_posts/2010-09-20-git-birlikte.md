---
layout: post
title: GitHub'ta Birlikte Nasıl Çalışılır?
---

Bir yazılım ekibinin Git sürüm takip sistemini kullanarak ortak çalışması için
pek çok senaryo var.   Bu dokümanda örnek bir senaryoda GitHub üzerinden sürecin
nasıl işlediğini özetleyeceğiz.  Senaryomuz şu şekilde gelişiyor:

*   `ecylmz` isimli geliştirici ([GitHub hesabı](http://github.com/ecylmaz)
    kendi hesabında `f` adında bir [proje](http://github.com/ecylmz/f)
    oluşturuyor.  Örneğin projeye aşağıdaki adresten erişebildiğini kabul
    edelim:

    [http://github.com/ecylmz/f](http://github.com/ecylmz/f)

*   `roktas` isimli geliştirici ([GitHub hesabı](http://github.com/roktas)) bir
    aşamada projeye dahil olarak `ecylmz` ile birlikte çalışmaya başlıyor.

### `roktas` proje üzerinde çalışmaya nasıl başlamalı?

Projeyi oluşturan kişi `ecylmz` olduğuna göre `roktas` öncelikle `f` isimli
projeyi `ecylmz`'ın deposundan "fork" ederek almalı.  Bu "fork" işlemi
için GitHub'ın [yardım sayfasında](http://help.github.com/forking/) ayrıntılı
bilgi bulabilirsiniz.  Buna göre:

*   `roktas` `ecylmz`'ın hesabındaki [proje](http://github.com/ecylmz/f)
    sayfasına giriyor.

*   Proje sayfasında görülen "Fork" butonuna tıklayarak projeyi kendi hesabına
    klonluyor.  Artık `roktas`'ın hesabında `f` isimli bir
    [proje](http://github.com/roktas/f) var.

*   `roktas` GitHub üzerinde oluşturduğu bu yeni depoyu makinesine alıyor:

    ```sh
    $ git clone git@github.com:roktas/f.git
    ```

*   `roktas`, kendi kopyası üzerinde çalıştığı esnada `ecylmz`'ın yaptığı yeni
    değişiklikleri takip etmek için deposunda örneğin `ecylmz` isimli bir uzak
    başvuru oluşturarak bunu ilkliyor:

    ```sh
    $ git remote add ecylmz git://github.com/ecylmz/f.git
    $ git fetch ecylmz
    ```

    Uzak başvuru adı olarak `ecylmz` dışında bir isim de seçilebilir.  Burada
    önerilen yöntemin avantajı birden fazla uzak başvuru kullanıldığında her
    birinin hangi kaynaktan geldiğinin daha açık şekilde anlaşılabilmesidir.

*   `roktas` proje üzerinde bilinen şekilde çalışıyor;  kodlama yapıyor,
    değişikliklerini kaydediyor ve GitHub'taki deposuna gönderiyor:

    ```sh
    $ git commit -a
    $ git push origin master
    ```

### `roktas` projede yaptığı değişiklikleri `ecylmz`'a nasıl göndermeli?

`roktas`'ın elindeki kopya GitHub'ın "fork" özelliğiyle `ecylmz`'ın kopyasından
türetildiğinden, GitHub `roktas`'a yaptığı değişiklikleri `ecylmz`'a iletmek için
"Pull Request" adında bir kolaylık sunuyor.  Buna göre:

*   `roktas` kendi [proje](http://github.com/roktas/f) sayfasına girerek "Pull
     Request" butonuna tıklıyor.

*    Açılan ileti penceresindeki ileti alanına uygun bir açıklama yazdıktan
     sonra `ecylmz`'a ve isteğe bağlı olarak diğer alıcılara "yaptığım değişiklikleri
     kendi deponuza alın" isteğinde bulunuyor.

*   Bu isteği alan `ecylmz` `roktas`'ın değişikliklerini kendi kopyasına
    uyguluyor.

### `roktas` `ecylmz`'ın değişikliklerini nasıl takip etmeli?

`roktas` proje üzerinde çalışırken `ecylmz` da kendi kopyası üzerinde bazı
değişiklikler yapıyor olabilir.  `roktas` bu değişiklikleri şöyle alıyor:

*   `roktas` kendi kopyasını hazırlarken oluşturduğu `ecylmz` dalını kullanarak
    `ecylmz`'daki değişiklikleri indiriyor:

     ```sh
     $ git fetch ecylmz
     ```

*   İndirilen değişiklikleri kendi kopyasıyla birleştiriyor:

    ```sh
    $ git merge ecylmz/master
    ```
