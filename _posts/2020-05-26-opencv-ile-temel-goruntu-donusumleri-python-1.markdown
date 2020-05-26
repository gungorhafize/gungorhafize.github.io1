---
layout: post
title: OpenCv ile Temel Geometrik Görüntü Dönüşümleri - 1 | Python
date: 2020-05-26 00:00:00 +0300
description: 
img: how-to-start.jpg # Add image post (optional)
tags: [Programming, Learn] # add tag
---

> Geometrik dönüşümler görüntü üzerindeki her pikselin bir konumdan(x,y), başka bir konuma (x2,y2) haritalanmasıdır.

Affine transformation’dan bahsetmeden önce öklit dönüşümüne bir göz atalım isterim. Öklit dönüşümleri uzunluk ve açı ölçüsünü koruyan bir tür geometrik dönüşümlerdir. Yani biz bir geometrik şekli alır ve ona öklit dönüşümü uygularsak değişmeden kalacaktır.
Dönmüş, kaymış, yamulmuş gibi görünebilir ancak temel yapısı değişmez yani bir doğru parçası ise doğru şeklinde, kare ise geometrik şeklimiz kare olarak kalır. Affine dönüşümlerine ise Öklit dönüşümünün genelleşmiş hali diyebiliriz. Ancak burada dikkat
edilmesi gereken nokta affine dönüşümlerinde uzunluk ve açılar korunmaz yani bir doğru parçası yine affine dönüşümden sonra doğru parçası olarak hayatına devam ederken, bir kare şeklimiz paralelkenar haline gelebilir.

Affine dönüşüm matrisi oluşturmak için kontrol noktalarını yani check pointleri tanımlamamız gerekir. Bu kontrol noktalarını tanımlandıktan sonra, bunların nerede haritalandırılmasını istediğimize karar vermeliyiz. Bu özel durumda, ihtiyacımız olan tek şey kaynak görüntüde üç nokta ve çıktı görüntüde üç noktadır. Bir görüntüyü paralelkenar benzeri bir görüntüye nasıl dönüştürebileceğimizi görelim:

```python
import cv2
import numpy as np

img = cv2.imread('pool.png')
rows, cols = img.shape[:2]

src_points = np.float32([[0,0], [cols-1,0], [0,rows-1]])
dst_points = np.float32([[0,0], [int(0.6*(cols-1)),0], [int(0.4*(cols-1)),rows-1]])
affine_matrix = cv2.getAffineTransform(sourc_points, dest_points)
img_output = cv2.warpAffine(img, affine_matrix, (cols,rows))

cv2.imshow('Input', img)
cv2.imshow('Output', img_output)
cv2.waitKey()
```
![affinetrans](https://user-images.githubusercontent.com/33956266/82947879-6de16980-9fa9-11ea-8b48-ce240a9149ad.JPG)

Affine dönüşüm matrisini elde etmek için sadece üç noktaya ihtiyacımız var. Sourc_points içindeki üç noktanın dest_points içindeki karşılık gelen noktalarla eşlenmesini istiyoruz. Noktaları aşağıda gösterildiği gibi eşleştiriyoruz:

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/82948022-aed97e00-9fa9-11ea-8c13-e6801ee12317.jpg">
</p>

2x3 dönüşüm matrisini elde etmek için OpenCV’de getAffineTransform adlı bir fonksiyonumuz var. Affine dönüşüm matrisini elde ettikten sonra, bu matrisi 3x3 input görüntü matrisine uygulamak için warpAffine fonksiyonunu kullandık.
Giriş görüntüsünün ayna görüntüsünü de alabiliriz. Kontrol noktalarını aşağıdaki şekilde değiştirmemiz gerekiyor:

```python
sourc_points = np.float32([[0,0], [cols-1,0], [0,rows-1]])
dest_points = np.float32([[cols-1,0], [0,0], [cols-1,rows-1]])
```

<p align="center">
  <img width="460" height="300" src="https://user-images.githubusercontent.com/33956266/82948027-b0a34180-9fa9-11ea-94d4-820c699de68e.jpg">
</p>

![affinetrans2](https://user-images.githubusercontent.com/33956266/82948036-b6992280-9fa9-11ea-8472-1bfa8e0f948d.JPG)

Kontrol noktaları değiştikten sonra ayna görünütüsü elde edebildik. Affine dönüşümler güzel fakat bir de Perspective — Projective dönüşümler var ki bir sonraki yazımda ondan bahsediyor olacağım. Keyifli okumalar.🧐
