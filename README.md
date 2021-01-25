# Android_Review_12
ViewMode Pattern and LiveData as an observable data holder


1. app's architecture =>


       app 
           - viewmodels 
             (to resolve the problems from I/O threads and UI main thread using coroutines instead of threads)
       
           - db - DBEntities.kt (DBVideo data class) (asDomainModel called Video)
                - Room.kt (wait to code)
           - domain - Models.kt (Video)
           - repo ((wait to code)
           
           

1. add dependencies using implementation method called in path app/build.gradle

        dependencies {

            // architecture components
            def lifecycle_version = "2.2.0"
            implementation "android.lifecycle:lifecycle-extensions:$lifecycle_version"
            implementation "android.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
            
            // coroutines for getting the UI thread
            def coroutines ="1.2.1"
            implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:$coroutines"
            implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:$coroutines"

        }

2. code for ViewModels Pattern using LiveData Module.
 
       // got to app/src/main/java....../katesvideoapp/viewmodels/KatesViewModel.kt
       
       package com.example.android.katesvideoapp.viewmodels
       
       [app module]
       import android.app.Application
       
       [lifecycle module]
       import androidx.lifecycle.AndoridViewModel
       import androidx.lifecycle.LiveData
       import androidx.lifecycyle.MutableLiveData
       import androidx.lifecycle.ViewModelProvider
       import androidx.lifecycle.viewModelScope
       
       [mediator data modules]
       
       [Domain data modules]
       
       [asDomain data modules]
       
       [local memory data models]
       
       [coroutines]
       
       [exception handler]


3. supplement for Android_Review_11. To create a Dao, also known as Data Access Object between DBVideo and VideosDB. to create a persistent DB model using Room. R/W from DBVideo to VideosDB.

          // TODO

          // go to app/src/main/java/..../katesvideoapp/db/Room.kt 持續性資料庫

          package com.example.android.devbyteviewer.db

          import androidx.room.*

          [modules matters with Livedata]

          [context related module]

          // TODO
  
