# Android_Review_12
ViewMode Pattern and LiveData as an observable data holder


1. app's architecture =>


       app 
           - viewmodels 
             (to resolve the problems from I/O threads and UI main thread using coroutines instead of threads)
       
           - repo (wait to code as a Mediator)
       
           - db   
                  - DBEntities.kt (DBVideo data class) (asDomainModel() method)
                 
                  - Room.kt (VideosDB entities) (getDB() method) (Dao retrieving data from DBVideo)
                 
           - domain 
                
                   - Models.kt (Video)
                   
           - network
           
                  - Service.kt (Network object)
           
           
  
  
2. MVC pattern
  
       (1) 遠端資料處理                                  (2) 手機系統內部緩存
       Retrofit, a web service. see Android_Review_10    Room, a persistent data models saved in Caches of the app.
       
       
             Network object
             
                   |
                   |
                   |
             
           data class in json format
            List<NetworkVideo>
           
             |          |                      
             |          |
             |        asDBModel()
             |          |                       
             |          |       
             |
             |
             |              data class constructor         interface        abstract class
             |               List<DBVideo>   -------------  Dao   --------   VideosDB  ---  Room
             |  
             |   
             |                                  |
             |                                  |
             |    
             | 
             |                 List<DBVideo>.asDomainModel(): List<Video>
             |                 
             |                                   |
             |                                   |
       asDomainModel()                           V
             |                  
             |-------------------------->      List<Video> 
                           
    
   
                             |                                                 |
                             |                                                 |
                             V                                                 V
                             
                             
  
                                 (3) 硬碟中的暫存器，扮演記憶體使用佔用資料的協調者角色
                                     Repository, a Mediator for data src from remote or local.
                                     If this data is stale, the app's repository module starts updating the data in the background.
                                  
                                                  |
                                                  |
                                                  
                                                  
                                      LiveData acts as an Event Observer
                                                  
                                                  
                                                  
                                                  |
                                                  V
                          
  
                                           (4) ViewModel
                                              to bind (3) with UI element. 
                                              M + V = C, 
                                              see Android_Review_12 using coroutines as workManager.
           

3. add dependencies using implementation method called in path app/build.gradle

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

4. code for Repository.

       // go to app/src/main/java/...../katesvideoapp/repo/VideosRepo.kt
       
       package com.example.katesvideoapp/repo
       
       [lifecycle module]
       import androidx.lifecycle.LiveData
       
       [cached data modules]
       import com.example.android.katesvideoapp.db.VideosDB // 緩存中的資料
       import com.example.android.katesvideoapp.db.asDomainModel // 緩存中資料轉型為準備使用的資料
       
       [coroutines modules]
       import kotlinx.coroutines.Dispatchers
       import kotlinx.coroutines.withContext
       
       [network module]
       import com.example.android.katesvideoapp.network.Network
       import com.example.android.katesvideoapp.network.asDBModel // 線上的資料轉型
       
       import com.example.android.katesvideoapp.network.Network
       
       [log]
       import timber.log.Timber
       
       /**
        * Repository fetches data from Network and storing them on Disk.
        **/

       class VideosRepo(){
       
       
             // using Domain Data Model called <Video>
             val videos: LiveData<List<Video>> 
       
       
       
             suspend fun refreshVideos(){
             
                    withContext(Dispatchers.IO){
                    
                         val playList = Netwrok.bytes.getPlayList()
                         db.videoDao.insert(playList.asDBModel())
                    
                    }
             
             }
       
       
       }





5. code for ViewModels Pattern using LiveData Module. ViewModel is designed to store and manage UI-related data in a lifecycle concious way. It allows data to survive config changes such as Screen rotaions. And worker threads in background work such as fetching data from remote thru config and pass data after the Activity or Fragment is available.
 
       // got to app/src/main/java....../katesvideoapp/viewmodels/KatesViewModel.kt
       
       package com.example.android.katesvideoapp.viewmodels
       
       [app module]
       import android.app.Application
       
       [lifecycle module]
       import androidx.lifecycle.AndoridViewModel
       import androidx.lifecycle.LiveData
       import androidx.lifecycyle.MutableLiveData
       
       [ViewModel Modules]
       import androidx.lifecycle.ViewModelProvider
       import androidx.lifecycle.viewModelScope
       
       [mediator data modules]
       import com.example.andorid.katesvideoapp.repo.VideosRepo
       
       [Domain data modules]
       import com.example.andorid.katesvideoapp.domain.Video
       
       [asDomain data modules]
       import com.example.andorid.katesvideoapp.network.asDomainModel
       
       [local memory data models]
       import import com.example.andorid.katesvideoapp.db.getDB
       
       [coroutines]
       import kotlinx.coroutines.*
       
       [exception handler]
       import java.io.IOException
       
  
       class ViewModel(application: Application): AndroidViewModel(application){
       
              // TODO
              // 資料常數宣告定義如下
              
              
              
              // after ViewModel is created, then this method is called immediately
              init {
              
                 funcCalled()
              
              }
              
              private fun funCalled(){
              
                   viewModelScopr.launch {
                   
                     // TODO: body
                     // try{}catch(networkErr: IOException){}
                   
                   }
              
              }
              
              
              class 
       
       }


6. supplement for Android_Review_11. To create a Dao, also known as Data Access Object between DBVideo and VideosDB. to create a persistent DB model using Room. R/W from DBVideo to VideosDB.

          // from DBVideo to VideosDB
          // go to app/src/main/java/..../katesvideoapp/db/Room.kt 持續性資料庫

          package com.example.android.devbyteviewer.db

          [local cache]
          import androidx.room.*

          [LifeCycle modules matters with Livedata]
          import androidx.lifecycle.LiveData

          [context related module]
          import android.content.Context

          // Data Access Obj 
          @Dao 
          interface VideoDao {
          
                 @query
                 fun getVideos(): LiveData<List<DBVideo>>
                 
                 @insert insert(videos: List<DBVideo>)
          
          }
          
          
          // DB
          @Database(entities = [DBVideo], version = 1)
          abstract class VideosDB: RoomDatabase(){
          
             abstract val videoDao:VideoDao  
          
          }
          
          /**
           * Refresh the videos stored in the offline Cache (線下緩存).
           *
           *
           * This function uses the IO dispatcher to ensure the DB insert DB ops 
           * heppens on the IO dispatcher.
           *
           * By switching the IO dispatcher using `withContext`
           * this function is safe to call from any thread including the Main UI thread & those Work threads/
           *
           *
          **/
          
          
          // public method to get DB entities
          
          private lateinit var INSTANCE: VideosDB
          
          fun getDB(conext: Context): VideosDB {
         
             // an inspector
          
             return INSTANCE
          
          }
          
  6. inspector 驗證寫法 in public method called getDB().
  
     https://github.com/google-developer-training/android-kotlin-fundamentals-apps/blob/master/DevBytesRepository/app/src/main/java/com/example/android/devbyteviewer/database/Room.kt
  
