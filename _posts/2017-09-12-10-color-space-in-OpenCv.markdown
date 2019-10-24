---
layout: post
title: Opencv ile Görüntü İşleme | Renk Uzayı
date: 2017-09-12 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: red-green-and-blue-eye.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Python, Opencv, colorspaces, color, hsv, rgb] # add tag
---

Görüntüler, bellekte farklı renk uzaylarında saklanır. Renk uzaylarından önce, OpenCV’nin veya başka herhangi bir programın (API) görüntüleri RAM’de nasıl sakladığını tam olarak bilmekte fayda var.
Basit bir şekilde gri tonlamalı bir görüntü düşünelim. Gri tonlamalı bir resim yalnızca yoğunluk bilgisi gerektirir yani belirli bir pikselin ne kadar parlak olduğu bilgisi. Değer ne kadar yüksek olursa yoğunluk o kadar yüksektir.

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/67438459-38803200-f5fc-11e9-96a4-645184067c89.png">
</p>
  
Gri tonlamalı bir görüntü için ihtiyacınız olan tek şey, her piksel için bir bayt. Bir bayt veya 8 bit, 0 ile 255 arasında bir değer depolayabilir ve tüm olası gri tonlarını kapsar. Böylece, bellekte gri tonlamalı bir görüntü iki boyutlu bir bayt dizisi ile temsil edilir. Dizinin boyutu görüntünün yüksekliğine ve genişliğine eşittir. Teknik olarak, bu dizi “kanal” dır. Dolayısıyla, gri tonlamalı bir görüntünün yalnızca bir kanala sahip olduğunu. Ve bu kanal beyazların yoğunluğunu temsil ediyor.

![gri2](https://user-images.githubusercontent.com/33956266/67438490-4cc42f00-f5fc-11e9-898c-08310eea2a46.jpeg)

Renk eklendiğinde ise işler daha da zorlaşıyor. Artık bellekte daha fazla bilgi saklanmakta… Saydam olmayan bir görüntü 16,581,375 (yaklaşık 16 milyon civarında) farklı rengi destekler. Bu farklı renk tonlarını ayırt edebilmek için, her piksel için 3 bayta ihtiyacımız var ( veya 24 bit). Şimdi şunu düşünelim; farklı renk tonlarına atamak için 16 milyon numaramız var. Her sayıya rastgele renk atamış ise, işler git gide garipleşir. Örneğin, 1= en parlak kırmızı, 2 = en parlak yeşil, 95760 = en koyu sarı… gibi. Böylece insanlar sistematik olarak yaklaşık 16 milyon renk sayılarına numaralar atamak için farklı “renk uzaylarını” buldular.
Bilgisayarla görü ve görüntü işlemede, renk uzayı, renkleri düzenlemenin bir yolunu ifade eder. Renk uzayı aslında renk modeli ve haritalama fonksiyonun birleşimidir. Renk modellerini istememizin aslında nedeni, tuples kullanarak piksel değerlerini temsil etmemize yardımcı olmasıdır. Haritalama fonksiyonu renk modelini, gösterilebilecek tüm olası renk kümeleriyle eşleştirir.
OpenCV’de 150'den fazla renk uzayı dönüştürme yöntemi bulunmaktadır. En popüler renk uzaylarından bazıları RGB nam-ı diğer BGR, YUV, HSV…
En yaygın renk uzayı RGB: Her piksel için 3 bayt veri 3 farklı bölüme ayrılmıştır: kırmızı miktar için bir bayt, yeşil için bir ve üçüncüsü mavi için bir bayt… Ana renkler olan kırmızı, yeşil ve mavi herhangi bir rengi oluşturmak için farklı oranlarda olabilir. 256 farklı kırmızı, yeşil ve mavi tonumuz vardır. 1 bayt, 0 ile 255 arasında bir değer saklayabilir. Böylece bu renkleri farklı oranlarda karıştırır ve istediğiniz rengi elde etmiş oluruz. Bu renk uzayı oldukça sezgiseldir.

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/67438510-64031c80-f5fc-11e9-908b-8503a36f7c77.png">
</p>

![rgb2](https://user-images.githubusercontent.com/33956266/67438533-7a10dd00-f5fc-11e9-8732-9a260b3c6511.jpeg)

OpenCV, en gelişmiş teknikleri kullanarak Computer Vision için harika bir kütüphanedir ve Windows, Linux, Mac ve hatta iPhone dahil olmak üzere birçok İşletim Sisteminde kullanılabilir. Ana renk formatı RGB’dir fakat aslında RGB’nin baytları olan ve verimlilik için 4. byte’lık bir bayt olan BGRX’dir ancak genel olarak basitlik için RGB olarak adlandırılır. Ayrıca RGB’den HSV’ye veya YUV veya LAB veya XYZ’ye dönüştürmek gibi farklı renk formatlarıyla çalışmak için çeşitli fonksiyonlar da içerir. Ancak OpenCV’nin HSV formatı beklediğinizden farklı!
OpenCV’nin RGB’den HSV’ye ve HSV’den RGB’ye dönüşümü, Hue bileşenini yalnızca 0 ila 255’i kolayca kullanabileceği durumlarda 0 ila 179 aralığında 8 bitlik bir tam sayı olarak depolar. OpenCV’nin cvCvtColor’unu kullanmak durumundayız. Bir HSV görüntüsü elde etmek için, renk tonunun bir kısmını kaybedeceksiniz, çünkü temelde Hue 8 bitlik bir sayı yerine 7 bitlik bir sayı olarak saklanıyor. Ayrıca, bu HSV görüntüsünü diğer yazılımlardan veya kütüphanelerden bulduğunuz renk değerlerini temel alarak işlerseniz, çoğu sistemde 0–255 aralığında kullandığınızdan, sorun çıkması olasıdır. Örneğin, bir cilt dedektörü yazıyorsanız HSV değerlerini okumak için herhangi bir görüntü düzenleyici (MS Paint veya Adobe Photoshop gibi) kullanırsanız, bunlar OpenCV için ihtiyacınız olan değerlerden farklı olacaktır.
Ayrıca, RGB renkler HSV renk uzayına birkaç farklı yolla dönüştürülebilir ve aynı renk için oldukça farklı değerler elde edilebilir. Yani OpenCV’de 255 doygunluğu her zaman parlak bir renktir, oysa çoğu yazılımda saf beyazdır. Bu, OpenCV ile başka yazılımların kullanılmasını daha da zorlaştırır, çünkü OpenCV’de kullanılan HSV formatı, çoğu görüntü düzenleme yazılımında kullanılan formattan farklıdır.
RGB birçok amaç için iyi olsa da, birçok gerçek yaşam uygulaması için çok sınırlı olma eğilimindedir. İnsanlar, yoğunluk bilgisini renk bilgisinden ayırmak için farklı yöntemler düşünmeye başladılar. Böylece, YUV renk uzayı ile geldiler. Y, parlaklığı veya yoğunluğu ifade eder ve U/V kanalları renk bilgisini temsil eder. Bu, birçok uygulamada iyi çalışır, çünkü insan görsel sistemi yoğunluk bilgisini renk bilgisinden çok farklı algılar.
YUV bile bazı uygulamalar için hala yeterli değildi. Böylece insanlar renkleri nasıl algıladıklarını düşünmeye başladılar ve HSV renk uzayını oluşturdular. HSV, Ton, Doygunluk ve Değer anlamına gelir. Bu, renklerin en temel özelliklerinden üçünü ayırdığımız ve farklı kanallar kullanarak onları temsil ettiğimiz silindirik bir sistemdir. Bu, insan görsel sisteminin rengi nasıl anladığıyla yakından ilgilidir. Bu bize görüntüleri nasıl idare edebileceğimiz konusunda esneklik sağlar.

![hsv](https://user-images.githubusercontent.com/33956266/67438558-8a28bc80-f5fc-11e9-9e13-c37ea8727a96.jpeg)

Renk uzayları arasında dönüştürmek için cvtColor fonksiyonunu kullanıyoruz. İlk argüman giriş görüntüsüdür ve ikinci argüman renk alanı dönüşümünü belirtir.
