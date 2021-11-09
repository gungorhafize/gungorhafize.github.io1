---
layout: post
title: Android Jetpack Navigation Bileşeni |SafeArgs - 2
date: 2020-09-22 00:00:00 +0300
description:
img: nav2.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Android, Jetpack, Navigation, SafeArgs, kotlin] # add tag
---

Bu bölüm, Navigation bileşeni tarafından hedefler arasında kolayca veri aktarımı için sağlamak ve SafeArgs üzerinedir. Uygulamanızda farklı hedeflere giderken veri iletmek isteyebilirsiniz. 
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
  <img width="300" height="400" src="https://user-images.githubusercontent.com/33956266/140910270-e77120e8-89a2-4ecc-88f0-dbb2eb23e1da.PNG">
</p>

Bir bağımsız değişkene ihtiyaç duyan hedef, hangi öğenin görüntüleneceği hakkında bilgi gerektiren itemEntryDialogFragment iletişim kutusudur. Bu hedefe tıklamak, sağdaki hedef özelliklerini gösterdi. 



<p align="center">
  <img width="800" height="500" src="https://user-images.githubusercontent.com/33956266/140911158-dc575277-4604-4605-882c-15a55d592152.PNG">
</p>


  
  
  
  
  
  
