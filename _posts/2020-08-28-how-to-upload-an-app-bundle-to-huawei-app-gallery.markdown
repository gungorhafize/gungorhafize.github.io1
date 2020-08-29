---
layout: post
title: How to upload an app bundle (.aab file) to Huawei AppGallery | Huawei App Signing
date: 2020-08-28 00:00:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: red-green-and-blue-eye.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Huawei, AppGallery, app-signing, app-bundle, android, apk] # add tag
---

When you want to upload an android application to Huawei AppGallery, you must first obtain an account and log in.
You need to create the application you will install in AppGallery Connect.
# 1) Log into AppGallery Connect and click "Apps".

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/91637805-b6c71500-ea13-11ea-94b8-108555182358.png">
</p>

# 2) Click on "New App".

<p align="center">
  <img width="2000" height="100" src="https://user-images.githubusercontent.com/33956266/91637851-0efe1700-ea14-11ea-9d10-1038c1e4755e.png">
</p>

Then, after selecting the options such as application name, category, default language in the window that opens, we move on to the next step.

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/91637889-4240a600-ea14-11ea-8e21-77adb83052bf.png">
</p>

The point to note here is that "Add to Project" should be marked. This part will be required for the SHA-256 finger-print certificate in the next steps.
 
# 3) On the Distribute tab, go to Version Information > Draft.
After completing the app information section, in the software version area, click "Software Packages" and upload the software package. We can upload apk or .aab file here, but it would be a more logical option for you to install app bundle in terms of efficiency and optimization.

<p align="center">
  <img width="2000" height="300" src="https://user-images.githubusercontent.com/33956266/91638024-35708200-ea15-11ea-9a3e-98323487fb04.png">
</p>

In order to add the app bundle, we click on the "Software packages" button and "Upload" from the window that opens. However, since our application has not been signed yet, there will be an expression stating that we need to switch to the App Signature module in the Services section on the left tab.

## 📝 App Signing 

Android apps use a private key for signing. To ensure reliable application updates, each private key has an associated public key certificate. Devices and services can use the public key certificate to check whether an application is from a trusted source. An application update is considered reliable only when the update signature matches the signature of the uploaded application and the update is performed. Here again, there are two options.

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/91638205-4c63a400-ea16-11ea-8b6f-5f2560391fb4.png">
</p>

The first one marked here is the recommended one so that AppGallery Connect can properly manage and maintain your application signature key, it is recommended that you use App Signing - App Signing service and use the key to sign your app for distribution. The service securely stores the signature key and secures your content even in case of key loss or theft.

The second option, on the other hand, may expose you to the risk of not being able to update your application if your key is lost or stolen. In this case, you will need to use a new package name to publish the updated application.
The signing process for a new app is slightly different than for a published app.


```python 
import cv2
import numpy as np
import matplotlib.pyplot as plt
img = cv2.imread('color.png',1)
b,g,r = cv2.split(img)
cv2.imshow('Blue Channel',b)
cv2.imshow('Green Channel',g)
cv2.imshow('Red Channel',r)
img=cv2.merge((b,g,r))
cv2.imshow('Merged Output',img)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/67438510-64031c80-f5fc-11e9-908b-8503a36f7c77.png">
</p>

![rgb2](https://user-images.githubusercontent.com/33956266/67438533-7a10dd00-f5fc-11e9-8732-9a260b3c6511.jpeg)

OpenCV, en gelişmiş teknikleri kullanarak Computer Vision için harika bir kütüphanedir ve Windows, Linux, Mac ve hatta iPhone dahil olmak üzere birçok İşletim Sisteminde kullanılabilir. Ana renk formatı RGB’dir fakat aslında RGB’nin baytları olan ve verimlilik için 4. byte’lık bir bayt olan BGRX’dir ancak genel olarak basitlik için RGB olarak adlandırılır. Ayrıca RGB’den HSV’ye veya YUV veya LAB veya XYZ’ye dönüştürmek gibi farklı renk formatlarıyla çalışmak için çeşitli fonksiyonlar da içerir. Ancak OpenCV’nin HSV formatı beklediğinizden farklı!
OpenCV’nin RGB’den HSV’ye ve HSV’den RGB’ye dönüşümü, Hue bileşenini yalnızca 0 ila 255’i kolayca kullanabileceği durumlarda 0 ila 179 aralığında 8 bitlik bir tam sayı olarak depolar. OpenCV’nin cvCvtColor’unu kullanmak durumundayız. Bir HSV görüntüsü elde etmek için, renk tonunun bir kısmını kaybedeceksiniz, çünkü temelde Hue 8 bitlik bir sayı yerine 7 bitlik bir sayı olarak saklanıyor. Ayrıca, bu HSV görüntüsünü diğer yazılımlardan veya kütüphanelerden bulduğunuz renk değerlerini temel alarak işlerseniz, çoğu sistemde 0–255 aralığında kullandığınızdan, sorun çıkması olasıdır. Örneğin, bir cilt dedektörü yazıyorsanız HSV değerlerini okumak için herhangi bir görüntü düzenleyici (MS Paint veya Adobe Photoshop gibi) kullanırsanız, bunlar OpenCV için ihtiyacınız olan değerlerden farklı olacaktır.
Ayrıca, RGB renkler HSV renk uzayına birkaç farklı yolla dönüştürülebilir ve aynı renk için oldukça farklı değerler elde edilebilir. Yani OpenCV’de 255 doygunluğu her zaman parlak bir renktir, oysa çoğu yazılımda saf beyazdır. Bu, OpenCV ile başka yazılımların kullanılmasını daha da zorlaştırır, çünkü OpenCV’de kullanılan HSV formatı, çoğu görüntü düzenleme yazılımında kullanılan formattan farklıdır.
RGB birçok amaç için iyi olsa da, birçok gerçek yaşam uygulaması için çok sınırlı olma eğilimindedir. İnsanlar, yoğunluk bilgisini renk bilgisinden ayırmak için farklı yöntemler düşünmeye başladılar. Böylece, YUV renk uzayı ile geldiler. Y, parlaklığı veya yoğunluğu ifade eder ve U/V kanalları renk bilgisini temsil eder. Bu, birçok uygulamada iyi çalışır, çünkü insan görsel sistemi yoğunluk bilgisini renk bilgisinden çok farklı algılar.
YUV bile bazı uygulamalar için hala yeterli değildi. Böylece insanlar renkleri nasıl algıladıklarını düşünmeye başladılar ve HSV renk uzayını oluşturdular. HSV, Ton, Doygunluk ve Değer anlamına gelir. Bu, renklerin en temel özelliklerinden üçünü ayırdığımız ve farklı kanallar kullanarak onları temsil ettiğimiz silindirik bir sistemdir. Bu, insan görsel sisteminin rengi nasıl anladığıyla yakından ilgilidir. Bu bize görüntüleri nasıl idare edebileceğimiz konusunda esneklik sağlar.

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/67438558-8a28bc80-f5fc-11e9-9e13-c37ea8727a96.jpeg">
</p>

Renk uzayları arasında dönüştürmek için cvtColor fonksiyonunu kullanıyoruz. İlk argüman giriş görüntüsüdür ve ikinci argüman renk alanı dönüşümünü belirtir.
