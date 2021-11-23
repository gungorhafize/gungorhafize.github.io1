---
layout: post
title: Serializable vs Parcelable? 
date: 2020-09-22 00:00:00 +0300
description:
img: svsp.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Android, Serializable, Parcelable, interface, kotlin, java] # add tag
---

Genellikle, uygulama geliştirirken verileri bir activity'den diğerine aktarmamız gerekir. Bu veri transferini gerçekleştirirken sıklıkla Intent ve Bundle yapısını kullanırız. Bu yöntem daha çok string, integer, double, float, boolean gibi primitive data type verilerin transferinde kullanılır. 
Bunun dışında eğer ki bir obje göndermek istersek? O zaman da karşımıza 2 yöntem çıkıyor. Bu yöntemlerden biri Serializable diğeri ise Parcelable.
Java Serializable ile Android Pacelable'ı karşılaştırmadan önce cevabını bulmamız gereken bir soru var.
“Android bileşenleri arasında nesne aktarımında, neden onları Serileştirilebilir veya Ayrıştırılabilir hale getirmek zorundayız?” 
Bir uygulama arka plandayken, düşük bellek durumunda işletim sistemi tarafından sonlandırılabilir.
İşletim sistemi, kullanıcı uygulamaya geri döndüğünde, uygulama için yeni bir süreç oluşturur. Bu nedenle, nesnenin bir örneğini Bundle'a geçirmekle, süreç değiştiğinde, nesnenin referansı yeni süreçte olmayacak ve o nesneyi kullanmak mümkün olmayacaktır. 
Ayrıca, PendingIntents durumunda, uygulama öldürülürse, PendingIntent'in kendisi kendisine verilen diğer işlemlerden kullanılabilir durumda kalacaktır.
Bu nedenle, işletim sistemi bir nesnenin referansını kaydetmek yerine değerlerini kaydetmelidir ve bu sadece o nesneyi Parcelable veya Serializable'a dönüştürerek mümkün olacaktır. 

## Serializable Nedir?

Serileştirilebilir yani Serializable, standart bir Java interface'idir. Android SDK'nın bir parçası değildir. Güzel kısmı sade ve basit kullanımı olmasıdır. 
Nesneleri serializable getirmek, bir nesnenin durumunu bir byte stream'e dönüştürmek anlamına gelir ve seri durumdan çıkarmak, bir bayt akışını nesnenin bir kopyasına geri döndürmek anlamına gelir. 

Serileştirilebilir Nesneler(Serializable-objects) ne için kullanılır?

 * Nesnelerin verilerini diskteki bir dosyada saklamak.
 * Oyunlarda, Serializable kullanılarak oyunun durumu diskteki bir dosyaya kaydedilebilir.
 * Ağ üzerinden veri gönderme.
 * Android'de nesneleri farklı bileşenler arasında aktarma. 

Sadece bu interface' i uygulayarak POJO'muz bir Aktiviteden diğerine geçişe hazır olacaktır. 
Aşağıdaki kod parçasında bu arayüzü kullanmanın ne kadar basit olduğunu görebilirsiniz. 

<script src="https://gist.github.com/gungorhafize/2ace890f77a52c6a5b1dc3e6ef6660aa.js"></script>


Bu kadar. Artık nesnenizi bir Android bileşenine gönderebilirsiniz. 
Tabii ki, her güzel şeyin bir bedeli olduğu gibi bu basit yaklaşımın bir bedeli var.
İşlem sırasında yansıma(reflection) kullanılır ve yol boyunca birçok ek nesne oluşturulur. Bu da, çok fazla garbage collection'a neden olabilir. Sonuç: düşük performans ve pil tüketimidir.

## Parcelable nedir? 

Parcelable da bir başka interface olarak karşımıza çıkmaktadır. Rakibine rağmen (hemen hatırlayalım Serializable -seri hale getirilebilir-), Android SDK'nın bir parçasıdır.
Parcelable, kullanılırken reflection olmayacak şekilde özel olarak tasarlanmıştır. Bunun nedeni, serileştirme süreci için gerçekten açık olmamızdır. 
Parcelable bir nesne oluşturmak, Serializable bir nesne oluşturmak kadar basit değildir. Öncelikle sınıfın Parcelabel interface'ini uygulaması ve gerekli tüm methodları override etmesi gerekir, ardından sınıfımızın Parcelable.Creator türünde CREATOR adında null olmayan bir statik alanı olmalıdır.
User sınıfımızı parcelable hale getirerek aşağıdaki gibi olur: 
Aşağıdaki kod parçasında, Parcelable interface'in örnek java kullanımını görebilirsiniz: 

<script src="https://gist.github.com/gungorhafize/2883673b8401adb1234ff975e3295f93.js"></script>

Kotlin kullanımını da "traditional" olarak aşağıda görebilirsiniz.

<script src="https://gist.github.com/gungorhafize/6dd13ff297b954f8089bb6cdcc11a6c4.js"></script>

"Lazy way" kotlin kullanımı da aşağıda gösterildi. :)

<script src="https://gist.github.com/gungorhafize/1c2b73213227aeaa1b7512a0d598c4d5.js"></script>

Bu kullanım için application düzeyinde build.gradle dosyasına kotlin-parcelize plugini eklememiz gerekiyor. 

Şimdi düelloya geçebiliriz. 

## Serializable vs Parcelable

<p align="center">
  <img width="500" height="400" src="https://user-images.githubusercontent.com/33956266/142142414-cf49e08f-030f-4854-834b-73ca94da6f48.PNG">
</p>

Yukarıda görülen Philippe Breault tarafından yürütülen testlerin sonuçları, Parcelable'ın Serializable'dan 10 kat daha hızlı olduğunu gösteriyor. Diğer bazı Google mühendisleri de bu açıklamanın arkasında duruyor. 
Serializable, Sadeliğin Ustası, Parcelable, Hız Kralı tanımı bence eğlenceli bir akılda kalıcılık.

İyi bir geliştirici olmak istiyorsak, 10 kat daha hızlı çalışacağı ve daha az kaynak kullanacağı için Parcelable'ı uygulamak için fazladan zaman ayırın.
Bununla birlikte, çoğu durumda, Serializable'ın yavaşlığı fark edilmeyecektir.Kullanmaktan çekinmeyin, ancak serileştirmenin pahalı bir işlem olduğunu unutmayın, bu nedenle minimumda tutun.

Binlerce serileştirilmiş nesne içeren bir listeyi iletmeye çalıştığımızı düşünürsek, tüm sürecin bir saniyeden fazla sürmesi olasıdır.
Portreden landscape mod geçişleri veya rotate işlemlerini çok ağır hissettirebilir. 


Şimdilik bu kadar. Tembel ve Üretken Kalın.

Referans için [tık][id] 

[id]: http://www.developerphil.com/parcelable-vs-serializable/  

