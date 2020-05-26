---
layout: post
title: OpenCv ile Temel Geometrik Görüntü Dönüşümleri - 1 | Python
date: 2020-05-26 00:00:00 +0300
description: 
img: how-to-start.jpg # Add image post (optional)
tags: [Programming, Learn] # add tag
---
Geometrik dönüşümler görüntü üzerindeki her pikselin bir konumdan(x,y), başka bir konuma (x2,y2) haritalanmasıdır.
Affine transformation’dan bahsetmeden önce öklit dönüşümüne bir göz atalım isterim. Öklit dönüşümleri uzunluk ve açı ölçüsünü koruyan bir tür geometrik dönüşümlerdir. Yani biz bir geometrik şekli alır ve ona öklit dönüşümü uygularsak değişmeden kalacaktır. Dönmüş, kaymış, yamulmuş gibi görünebilir ancak temel yapısı değişmez yani bir doğru parçası ise doğru şeklinde, kare ise geometrik şeklimiz kare olarak kalır. Affine dönüşümlerine ise Öklit dönüşümünün genelleşmiş hali diyebiliriz. Ancak burada dikkat
edilmesi gereken nokta affine dönüşümlerinde uzunluk ve açılar korunmaz yani bir doğru parçası yine affine dönüşümden sonra doğru parçası olarak hayatına devam ederken, bir kare şeklimiz paralelkenar haline gelebilir.
Affine dönüşüm matrisi oluşturmak için kontrol noktalarını yani check pointleri tanımlamamız gerekir. Bu kontrol noktalarını tanımlandıktan sonra, bunların nerede haritalandırılmasını istediğimize karar vermeliyiz. Bu özel durumda, ihtiyacımız olan tek şey kaynak görüntüde üç nokta ve çıktı görüntüde üç noktadır. Bir görüntüyü paralelkenar benzeri bir görüntüye nasıl dönüştürebileceğimizi görelim:



>Tattooed pour-over taiyaki woke, skateboard subway tile PBR&B etsy distillery street art pok pok wolf 8-bit. Vegan bicycle rights schlitz subway tile unicorn taiyaki.

Meditation literally adaptogen locavore artisan polaroid occupy sriracha bitters gochujang kale chips mixtape.

Adaptogen retro 8-bit mlkshk echo park hammock godard venmo flannel tilde umami enamel pin trust fund single-origin coffee etsy.

Skateboard keytar actually disrupt taiyaki, synth biodiesel. Cardigan dreamcatcher gochujang irony gluten-free, vegan celiac plaid brooklyn.
