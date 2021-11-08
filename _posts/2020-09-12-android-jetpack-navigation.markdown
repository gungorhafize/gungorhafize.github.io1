---
layout: post
title: Android Jetpack Navigation Bileşeni
date: 2020-09-20 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: nav2.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Android, Jetpack, Navigation, kotlin] # add tag
---

Günümüzde hemen hemen tüm mobil uygulamalar, son kullanıcılara gerekli tüm özellikleri sağlamak için birden fazla ekran içermektedir.
Splash Screen'den ana ekrana, kullanıcı bu ana ekrandan, diğer ekranlarda sonuç gösteren birçok farklı diğer görevleri gerçekleştirebilir. Bu ekranlar da genellikle uygulama içinde activity ve fragmentlerle gerçekleştirilir.  
Navigation, kullanıcıların uygulamanızdaki farklı içerik parçaları arasında gezinmesine, bu içeriklere girmesine ve bu içeriklerden geri çıkmasına olanak tanıyan etkileşimleri ifade eder.
Android Jetpack'in Navigation bileşeni, basit buton tıklamalarından app bar ve navigation drawer gibi daha karmaşık yapılara kadar gezinmeyi uygulamanıza olanak tanır.
Navigasyon bileşeni aynı zamanda yerleşik bir dizi ilkeye bağlı kalarak tutarlı ve öngörülebilir bir kullanıcı deneyimi sağlar. 
Navigasyon olmadan, karmaşık navigasyon yolları oluşturan bir ekrandan diğerine geçmek için bir çok büyük yer kaplayan manuel kod yazmamız gerekiyordu. Artık gerekmiyor diyebiliriz Jetpack Navigation bileşeni imdadımıza yetişti! :) 

**Navigasyon bileşeni, üç temel bölümden oluşur:

     * Navigation Graph: Navigasyon ile ilgili tüm bilgileri tek bir merkezi konumda içeren bir XML dosyası. Bu, uygulamanızdaki hedefler olarak adlandırılan tüm içerik alanlarının yanı sıra bir kullanıcının uygulamanızda izleyebileceği olası yolları içerir.
     * NavHost: Navigasyon grafiğinizden hedefleri görüntüleyen boş bir container gibi düşünebiliriz. Navigasyon bileşeni, fragment hedefini görüntüleyen varsayılan bir NavHost uygulaması olan NavHostFragment'i içerir.
     * NavController: Bir NavHost içinde uygulama navigasyonunu yöneten bir nesne. NavController, kullanıcılar uygulamanızda hareket ettikçe NavHost'taki hedef içeriğin değiştirilmesini düzenler. 

Özetleyecek olursak, çalışma mantığı; uygulamanızda gezinirken, NavController'a navigasyon grafiğinizdeki belirli bir yol boyunca veya doğrudan belirli gitmek istediğiniz hedef söylenir ve
NavController da daha sonra NavHost'a uygun hedefi gösterir. 
<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/140504984-380b4b9e-c225-49fa-a114-cb35f36b20bc.png">
</p>

**Android Jetpack Navigation Bileşeni Avantajları:
 * Fragment transaction asenkron olarak gerçekleşir.
 * Test etmek çok daha kolaydır.
 * Deep linkig kurmayı kolay ve tutarlı hale getirir.
 * Argüman geçirmek çok daha güvenlidir.
 * Varsayılan olarak doğru şekilde işleme ve geri alma.
 * Navigasyon sırasında bilgi aktarırken tip güvenliği sağlar.
 * Varsayılan geçiş animasyonlarını ayarlayabiliriz. 
 * Navigation UI pattern içerir (navigation drawers, bottom navigation vs).
 * Navigation Editor, hazırladığınız navigation graph’ı görmek veya düzenleyebilmek için kolaylık sağlar.
 * ViewModel desteği - UI ile ilgili verileri grafiğin hedefleri arasında paylaşmak için bir ViewModel'i bir navigasyon grafiğine dahil edebilirsiniz. 

Navigation grafiğini kullanmak için Android 3.3 veya sonraki bir sürümde olmanız gerekir. Yapmanız gereken ilk şey, gerekli bağımlılıkları bildirmektir. 

```kotlin
dependencies {
  val nav_version = "2.3.5"

  // Java language implementation
  implementation("androidx.navigation:navigation-fragment:$nav_version")
  implementation("androidx.navigation:navigation-ui:$nav_version")

  // Kotlin
  implementation("androidx.navigation:navigation-fragment-ktx:$nav_version")
  implementation("androidx.navigation:navigation-ui-ktx:$nav_version")

  // Feature module Support
  implementation("androidx.navigation:navigation-dynamic-features-fragment:$nav_version")

  // Testing Navigation
  androidTestImplementation("androidx.navigation:navigation-testing:$nav_version")

  // Jetpack Compose Integration
  implementation("androidx.navigation:navigation-compose:2.4.0-beta02")
}

```
