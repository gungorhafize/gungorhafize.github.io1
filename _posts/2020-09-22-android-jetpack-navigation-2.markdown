---
layout: post
title: SafeArgs | Android Jetpack Navigation Bileşeni-2
date: 2020-09-22 00:00:00 +0300
description:
img: nav22.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Android, Jetpack, Navigation, SafeArgs, kotlin] # add tag
---

Bu bölümde Navigation bileşeni tarafından hedefler arasında kolayca veri aktarımı için sağlamak ve SafeArgs üzerine duracağız. Uygulamanızda farklı hedeflere giderken veri iletmek isteyebilirsiniz. 
Verileri iletmek, global nesnelere referanslar kullanmak yerine, kodunuzda daha iyi kapsüllemeye izin verir, böylece farklı fragment veya activitylerin yalnızca kendilerini doğrudan ilgilendiren kısımları paylaşması gerekir. 
Navigation bileşeni, Android'de farklı activityler arasında bilgi iletmek için kullanılan genel mekanizma olan "Bundles" ile argümanların iletilmesini sağlar. Bunu burada tamamen yapabiliriz, iletmek istediğimiz argümanlar için bir Bundle oluşturabilir ve sonra onları diğer tarafta Bundle'dan çıkarabiliriz. 
Ancak Navigasyonun çok daha iyi bir özelliği var: **SafeArgs.**
SafeArgs, iletmek istediğiniz argümanlar hakkında navigation grafiğine bilgi girmenizi sağlayan bir gradle eklentisidir. 
Ardından, bu argümanlar için bir Bundle oluşturmanın ve diğer taraftaki bu argümanları Bundle'dan çıkarmanın sıkıcı kısımlarını ele alan sizin için kod üretir. 
Bundle'ı doğrudan kullanabiliriz ancak bunun yerine SafeArgs kullanmak Google tarafından önerilmektedir. Sadece yazması daha kolay olmakla kalmaz çok daha az kodla aynı zamanda argümanlarınız için tür güvenliği sağlayarak kodunuzu doğal olarak daha sağlam hale getirir. 

## SafeArgs nasıl çalışır?
Fab butona tıklayarak erişilen bir dialogda her biri ad, açıklama ve derecelendirme bilgilerine sahip ögelerin bir listesini gösteren bir uygulamamız olsun.
Her bir ögeyi güncellemek için kodun, tıklanan öğeyle ilgili bilgileri dialoga iletmesi gerekir. Özellikle, ögenin id'sini liste fragementinden diyalog fragmentine geçirmesi gerekir, ardından diyalog bu id'yw sahip item için veritabanından bilgi alabilir ve ardından formu uygun şekilde doldurabilir.
Veriyi(id) geçirmek için, SafeArgs kullanacağız. 
Her şeyden önce, bazı kütüphane bağımlılıklarına ihtiyacımız var

SafeArgs, navigationın diğer bölümleriyle aynı türde bir kütüphane modülü değildir; kendi başına bir API değil, kod üretecek bir gradle eklentisidir. Bu yüzden, onu bir gradle bağımlılığı olarak çekmem ve ardından gerekli kodu oluşturmak için eklentiyi derleme zamanında çalışacak şekilde uygulamamız gerekiyor.

Bunu önce projenin build.gradle dosyasının dependencies bloğuna ekledim: 

```kotlin
def nav_version = "2.3.0"
classpath “androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version”

```

Muhtemelen bunun yerine kullanabileceğiniz daha yeni bir sürüm olacaktır. Ardından uygulamanın build.gradle dosyasına aşağıdaki komutu ekledim. Bu, SafeArgs çağrıları için kodun oluşturulmasına neden olan parçadır. 

```kotlin
apply plugin: "androidx.navigation.safeargs.kotlin"

```
Ardından, gerekli verileri oluşturmak ve iletmek için navigasyon grafiğine gittim. 


<p align="center">
  <img width="500" height="400" src="https://user-images.githubusercontent.com/33956266/140910270-e77120e8-89a2-4ecc-88f0-dbb2eb23e1da.PNG">
</p>

Bir bağımsız değişkene ihtiyaç duyan hedef, hangi öğenin görüntüleneceği hakkında bilgi gerektiren itemEntryDialogFragment iletişim kutusudur. Bu hedefe tıklamak, sağdaki hedef özelliklerini gösterdi. 


<p align="center">
  <img width="800" height="500" src="https://user-images.githubusercontent.com/33956266/140911740-2fae694d-c1cb-4df9-92d0-7913f8fe0ebc.PNG">
</p>

Bir hedefe tıklamak, o hedefin özellik(attribute) sayfasını getirir; burası ona iletmek için argümanlar girebileceğiniz yerdir. 
Yeni bir arguments eklemek için Arguments bölümündeki + işaretine tıkladım, bu da aşağıdaki diyaloğu getirdi. Hangi item ögesinin görüntüleneceği hakkında bilgi vermek istedim, bu yüzden veri tabanındaki id type'a karşılık gelmesi için type olarak Long'u seçtim. 
  
  
<p align="center">
  <img width="300" height="300" src="https://user-images.githubusercontent.com/33956266/140912384-d5c49ec5-b745-4c68-9242-0d5dd480c710.PNG">
</p>  

Long'u seçtiğimizde Nullable ögesinin gri olduğunu görüyoruz. Bunun nedeni, izin verilen temel türlerin (Integer, Boolean, Float, and Long) Java programlama dili katmanında primitive data types (int, bool, float ve long) tarafından desteklenmesi ve bu türlerin boş olamaz durumudur.
Bu nedenle, Kotlin'in Long türü null yapılabilir olsa da, temeldeki primitive long türü null olamaz, bu nedenle bu temel türleri kullanırken null olmayan türlerle sınırlandırılırız. 

Unutulmaması gereken başka bir şey de, uygulamanın artık hem yeni bir öğe girmek, hem de mevcut bir öğeyi düzenlemek için dialog hedefini kullanmasıdır. Her zaman iletilecek bir itemId olmayacaktır; kullanıcı yeni bir öğe oluşturduğunda, kod görüntülenecek mevcut bir öge olmadığını belirtmelidir. Bu nedenle, -1 geçerli bir dizin olmadığı için, bu durumu belirtmek için iletişim kutusunda varsayılan değer için -1 girdim. Kod, herhangi bir bağımsız değişken sağlanmadan bu hedefe gittiğinde, varsayılan -1 değeri gönderilir ve alıcı kod, yeni bir item oluşturulduğuna karar vermek için bu değeri kullanır. 

Bu adımdan sonra Rebuild project yaptığımızda proje files listesindeki “java (generated)” dosyalarına giderek oluşturulan kodun sonuçlarını görebilirsiniz. Alt klasörlerden birinin içinde, argümanı iletmek ve almak için oluşturulan yeni dosyaları görebilirsiniz. 
ItemListDirections'da, dialog kutusuna gitmek için kullandığım API olan eşlik eden nesneyi görebilirsiniz. 

```kotlin
companion object {
    fun actionItemListToItemEntryDialogFragment(
        itemId: Long = -1L): NavDirections =
        ActionItemListToItemEntryDialogFragment(itemId)
}
```

Navigate() metodunun orijinal olarak kullandığı bir Action kullanmak yerine, hem eylemi (bizi diyalog hedefine götürür) hem de daha önce oluşturulan argümanı içine alan NavDirections nesnesini kullandık. 
  
  
