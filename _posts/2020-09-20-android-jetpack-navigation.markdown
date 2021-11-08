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
  <img width="800" height="500" src="https://user-images.githubusercontent.com/33956266/140744932-e2b94ae7-5486-46aa-9904-255faa4701e6.png">
</p>

Android Jetpack Navigation Bileşeni Avantajları:
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

Projeye navigation graph eklemek için resource dosyasına sağ tıklayıp yeni New > Android Resource File seçmeniz gerekmektedir. Gelen ekrandaki açılır listeden Resource type olarak Navigation seçin, dosyanın adını nav_graph ya da başka benzeri bir isimle kaydedin.

Oluşturduğumuz xml dosyasına tıkladığımızda karşımıza bir editör ekranı açılacak. Burada daha önce oluşturmuş olduğunuz fragmentleri destination olarak gösterebilir ya da editörün sol üstündeki + ikonuna tıklayarak orada yeni bir fragment oluşturabilirsiniz.

<p align="center">
  <img width="800" height="500" src="https://user-images.githubusercontent.com/33956266/140752907-90859bae-450b-4cc5-bbac-b63e3895e02d.png">
</p>

Artık sıra activity’e NavHost eklemeye geldi. Navigation component aslında bir activity ve birden fazla fragment’tan oluşan yapılar için tasarlanmıştır. Ana activity bir navigation graph ile ilişkilidir ve tabii ki hedefleri değiştirmekten sorumlu bir NavHostFragment içerir. Birden çok activity olan uygulamalarda fragment geçişleri için her activty’nin bir navigation graph’i olmalıdır.

Activity aşağıdaki gibi NavHostFragment içeren bir xml parçası bulundurmalıdır. <!Not: <fragment> tagi .FragmentContainerView'e update edilmiştir./>


```xml
 <androidx.fragment.app.FragmentContainerView
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"

        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />

```

Aşağıdaki örnek bir navigation graph yer almaktadır. Her bir ekran bir fragmentı, oklar ise action'ları temsil etmektedir.

<p align="center">
  <img width="800" height="500" src="https://user-images.githubusercontent.com/33956266/140752544-88b9d3dd-6cdd-4911-b4e7-d8a6a78fa4df.png">
</p>
  
Sırada action işlemlerini yapmak yani bir fragment’a nasıl gideriz. Her NavHost’un bir NavController’ı vardır actionları o yönetir. NavController’ı bulmak için aşağıdaki yöntemlerden birini kullanabilirsiniz.
  
Kotlin:

* Fragment.findNavController()
* View.findNavController()
* Activity.findNavController(viewId: Int)
  
Java:

* NavHostFragment.findNavController(Fragment)
* Navigation.findNavController(Activity, @IdRes int viewId)
* Navigation.findNavController(View)

  
NavController’a eriştikten sonra navigate() methodunu kullanarak geçilmek istenen fragment burada belirtilir. Burada dikkat edilmesi gereken bir nokta var. NavHost’unuzdaki belirtmiş olduğunuz navGraph’iniz geçmek istediğiniz fragmentları destination olarak belirlemelidir. Yoksa geçmek istediğiniz fragment’ı bulamayacak ve uygulamanız crash alıcaktır.
  
```xml
  Navigation.findNavController(view).navigate(R.id.action_title_screen_to_leaderboard)
```
  
  
