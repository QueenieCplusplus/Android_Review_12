# Android_Review_12
ViewMode Pattern and LiveData as an observable data holder

1. add dependencies using implementation method called in path app/build.gradle

        dependencies {

            // architecture components
            def lifecycle_version = "2.2.0"
            implementation "android.lifecycle:lifecycle-extensions:$lifecycle_version"
            implementation "android.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"

        }

2. code for ViewModels Pattern using LiveData Module.
 
       // got to app/src/main/java....../katesvideoapp/viewmodels/KatesViewModel.kt
       
       package com.example.android.katesvideoapp.viewmodels
       
       [app module]
       
       [lifecycle module]
       
       [data modules]
       
       [coroutines]
       
       [exception handler]
