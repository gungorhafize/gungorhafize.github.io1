---
layout: post
title: Android SDK Version | Çalışan activityler arasında iletişim kurma
date: 2017-09-12 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: android.png # Add image post (optional)
tags: [Productivity, Software] # add tag
---

Başka bir activity başlatmak ve ona bazı verileri iletmek Android'de basit ve temel bir işlemdir.
Ancak, zaten çalışan bir activity'nin ön plana gelmesini ve veri aktarmasını istiyorsanız, işler biraz karışabilir ve bu biraz zor olabilir.

Öncelikle, intenti olan activity'i çağırırsanız, başka bir instance zaten çalışıyor olsa bile o activityden yeni bir instance oluşturulur ve görüntülenir.
Bundan kaçınmak için de, activity'nin defalarca başlatılmaması gerektiği işaretlenmelidir. Ki bunu başarabilmek adına activty'nin launchMode'unu AndroidManifest.xml dosyasında singleTask yada singleTop olarak ayarlamalıyız.

🚀

```android
<activity android:name=".MainActivity"
            android:label="@string/app_name"
            android:launchMode="singleTop">
```

Bu şekilde bu activity'i intent kullanarak çağırdığımızda, mevcut bir instance varsa, sistem request'i buna yönlendirir.
Ancak, bu sefer iletilen extraData'yı yani yapmak istediğimiz işlemleri işlediğimiz onCreate methodu çalışmaz.

Adından da anlaşıldığı gibi, activity oluşturulduğunda çalışır (onCreate) ve bu sefer zaten var olduğundan, onNewIntent () adlı yöntem çağrılır.

```java
    @Override
    public void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        setIntent(intent);
        processExtraData();

    }
 ```
🧶
Iceland ramps readymade selfies synth ennui letterpress bushwick quinoa cred DIY VHS woke trust fund.

Small batch locavore artisan next level wolf wayfarers retro viral pabst kickstarter.🔮
✨

### Pour-over blue bottle woke listicle

Pour-over blue bottle woke listicle, post-ironic scenester poutine ennui four loko ramps kickstarter..

Everyday try-hard fanny pack thundercats farm-to-table.
