---
layout: post
title: Deep Dive into the Kotlin LiveData 🥂
date: 2023-01-22 00:00:00 +0300
description:
img: digital-id-google.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Android, LiveData, Kotlin, MutableLiveData, MediatorLiveData, transformations] # add tag
---

LiveData is a lifecycle-aware data holder that can be observed by UI components (like Activity or Fragment).
ViewModel can expose LiveData objects containing the necessary data for the UI.
UI components observe these LiveData objects, ensuring that they are always up to date with the latest ViewModel data.

## 🍎 Advantages of using LiveData:

 * Lifecycle Awareness: LiveData is aware of its observer's lifecycle state, ensuring updates are delivered only when the observer is active, thus preventing crashes and minimizing manual lifecycle management.
 * Safe Observer Configuration: LiveData automatically removes unused observers, preventing memory leaks and reducing manual cleanup efforts.
 * Efficient Thread Handling: LiveData in Android facilitates updates from background threads and automatically dispatches them to the main thread, removing the necessity for manual thread control.
 * Automated Updates: LiveData in Android automatically updates its observers, minimizing the need for manual updates and mitigating potential bugs from manual handling.
 * Immutable data: LiveData is designed to hold immutable data, which makes it easier to reason about and reduces the risk of bugs.
 * Effortless Testing: LiveData's separation of data and presentation concerns simplifies writing tests that primarily verify the displayed data, not the display mechanics.
 * Smooth Integration with Architecture Components: LiveData seamlessly meshes with key Android components like viewModel and Room, streamlining data management in your application.

Here's a practical example demonstrating the usage of LiveData in a simple scenario:

<script src="https://gist.github.com/gungorhafize/e0e885e0c6cf3f9ad5fc6b130be13f7d.js"></script>

In your activity or fragment, which are parts of the user interface (UI), you can implement the following:

<script src="https://gist.github.com/gungorhafize/934a5b63e1bbbb5ca6ff577e24ff46e3.js"></script>

In this example, within the MainViewModel, there exists a private mutable LiveData object named "_number" that can be modified. 
This private LiveData object is exposed as a public LiveData object named "number". 
The observe method is used to monitor changes in the data and update the associated TextView whenever the data changes.

## ⚔️ LiveData 🍎 vs MutableLiveData 🍏 ⚔️

LiveData is an observable data holder class that can be observed within a given lifecycle.
It is designed to be used as a holder for data that is meant to be exposed to the UI.
The data is observable, meaning that it can be observed by other components, such as an Activity or Fragment, for changes.
The data itself cannot be modified directly. 

MutableLiveData, is a subclass of LiveData that can be modified. It is a mutable data holder that can be observed within a given lifecycle.
It is recommended to use LiveData for data that is meant to be exposed to the UI, and to use MutableLiveData for data that is meant to be modified.
This helps to enforce a separation of concerns and promotes a clean architecture.

In this example below, MainViewModel has a public LiveData object, which is named number, cannot be modified directly.
<script src="https://gist.github.com/gungorhafize/6fef12968c881995adbdcd772a686a3f.js"></script>

In this code as shown in below, MainViewModel has a private mutable LiveData object which is named _number, exposed as a public LiveData object, number.
The increment method is used to modify the data by incrementing its value.
<script src="https://gist.github.com/gungorhafize/94ee1a54e88757e8335ea7f735a31959.js"></script>

## MediatorLiveData
MediatorLiveData is a subclass of LiveData that allows you to observe multiple LiveData objects and combine their values into a single LiveData object.
It acts as a “mediator” between multiple LiveData sources and provides a single source of truth for your UI.
Here are a few examples of how to use MediatorLiveData:

### 1) Merging LiveData sources: 
Suppose you have two LiveData sources, liveDataOne and liveDataTwo, and we want to observe the combined value of both sources in our UI.
We can use a MediatorLiveData to observe both sources and merge their values:
<script src="https://gist.github.com/gungorhafize/1fca7deb87a06077e6423a3e46846346.js"></script>
In the given example, the MainViewModel class contains two privately mutable LiveData instances, liveDataOne and liveDataTwo.
Additionally, there's a public accessible LiveData, named mergedData, which acts as a mediator. This mediator LiveData is constructed by observing both liveDataOne and liveDataTwo, combining their values into a single Pair object.
UI components can observe mergedData, and any alterations in liveDataOne or liveDataTwo will be automatically updated and reflected in the UI, providing real-time updates to the user interface based on changes in the observed LiveData sources.
## Using LiveData in Activity and Fragment 🖼️

The difference between using LiveData in an Activity versus a Fragment lies on the lifecycle management.



