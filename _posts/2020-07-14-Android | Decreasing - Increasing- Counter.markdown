---
layout: post
title: Android Decreasing Increasing Counter Design
date: 2020-07-14 14:41:20 +0300
description: 
img: i-rest.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Android, Counter, View, Design]
---
## Android Sayaç Görünümü Tasarımı
Bir sayaç tasarımı yapmak için önce bir tasarıma ihtiyaç duyarız. Activity_main.xml içerisine + / - butonları ve bir Textview eklemek sayacı oluşturmaya yetecektir.

👾

```xml
    <RelativeLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal">


        <Button
            android:id="@+id/btnDec"
            android:layout_width="50dp"
            android:layout_height="50dp"
            android:text="-"
            android:textStyle="bold"
            android:textColor="#fff"
            android:textSize="20sp"
            android:background="@drawable/round"
            android:layout_marginStart="5dp"
            android:layout_centerVertical="true"
            android:layout_alignParentLeft="true"
            android:visibility="gone"
            android:layout_marginLeft="60dp">

        </Button>

        <TextView
            android:id="@+id/value"
            android:layout_width="200dp"
            android:layout_height="50dp"
            android:text="1"
            android:textSize="30dp"
            android:textAlignment="center"
            android:background="@drawable/transparent"
            android:layout_marginStart="30dp"
            android:visibility="gone"
            android:layout_marginLeft="100dp"
            android:gravity="center_horizontal"></TextView>


        <Button
            android:id="@+id/btnInc"
            android:layout_width="50dp"
            android:layout_height="50dp"
            android:text="+"
            android:textStyle="bold"
            android:textColor="#fff"
            android:textSize="20sp"
            android:background="@drawable/round"
            android:layout_centerVertical="true"
            android:visibility="gone"
            android:layout_marginLeft="200dp"
            ></Button>

    </RelativeLayout>
    <View
        android:layout_width="wrap_content"
        android:layout_height="10dp"></View>

    <Button
        android:id="@+id/btnSave"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="Kaydet"
        android:textColor="#fff"
        android:background="@drawable/round"
        android:visibility="gone">

    </Button>

```

>👾 Drawable folder içerisinde bir view ekliyoruz.


```xml
  <?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle"
    android:padding="5dp">
    <solid android:color="#29B6F6"/> <!-- Yuvarlak buton renk mavi kodu-->
    <corners
        android:bottomRightRadius="50dp"
        android:bottomLeftRadius="50dp"
        android:topLeftRadius="50dp"
        android:topRightRadius="50dp"/>
</shape>

```
Textview için transparent bir view görünüm elde etmek için Drawable folder içerine transparent.xml ekliyoruz.


```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle"
    android:padding="5dp">
    <solid android:color="#E3F2FD"/> <!-- Transparent blue color -->
    <corners
        android:bottomRightRadius="50dp"
        android:bottomLeftRadius="50dp"
        android:topLeftRadius="50dp"
        android:topRightRadius="50dp"/>
</shape>

```
👾 Tasarım işlemleri tamamlandıktan sonra MainActivity içerisinde onCreate methodunda bu componentleri tanımlayıp ve click eventleri içine artırma azaltma işlemlerini uyguluyoruz.

```java
                value = findViewById(R.id.value);
                btnInc = findViewById(R.id.btnInc);
                btnDec = findViewById(R.id.btnDec);
                
                  btnInc.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        count++;
                        value.setText(String.valueOf(count)); //Value is a Textview
                    }
                });

                btnDec.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        if (count<=1) count=1;
                        else count--;
                        value.setText(String.valueOf(count));
                    }
                });
```


Burada count'un 1'den küçük olmamasına dikkat edildi. Basit bir view tasarımı elde etmiş olduk. Bir başka yazıda görüşmek üzere...😎
