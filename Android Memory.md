# Something For Android Memory
> reference by [Android Performance Patterns](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc9CBxr3BVjPTPoDPLdPIFCE)
> Knowledge for Android Render Performance , Memory and Garbage Collection

## Understanding Android Threading
寻找合适的帮手帮助更新UIThread，下列四个各司其职，各有各的场景
- AsyncTask
  > helps get work on/off the UI thread
- HandlerThread
  > Dedicated thread for API callbacks
- ThreadPool
  > Running lots of paralled work
- IntentService
  > Helps get intents off the UI thread
  
tools [Systrace](https://developer.android.com/studio/profile/systrace.html?utm_campaign=app_series_systrace_021816&utm_source=gdev&utm_medium=yt-annt)
you can use this to help you analysis your thread 
## Thread and Memory
## Good AsyncTask Hunting

- Basis
- Some Misunderstanding
- To Cancel some work
    - Check a "canceled" flag
    - Report work results invalid
  
  Inner class contains Implicit reference to outer Activity
  下面一种做法会导致隐式引用，请避免
  ```
  
  public class MainActivity extends Activity{
      // 这样写，由于Activity的生命周期长于
      public class MyAsyncTask extends AsyncTask<Void,Void,String> { 
          @Override protected String doInBackground(Void... params){...}
          @Override protected void onPostExecute(String result) {...}
      }
  }
  
  ```
  
## HandlerThread
> HandlerThread is for a long-running thread that grabs(抓住) work from a queue and operates on it
  
## Swimming in Threadpools

- ThreadPoolExecutor
> ThreadPoolExecutor is for 处理大量耗时操作
## The Zen of IntentService
## Thread and Loaders

- loaders
  > Loaders are wise to the inner workings of activity lifecycle, so you can ensure your work ends in the right place every time
  
## Do not leak views

- 不要引用Async Callbacks(异步回调函数)内部的视图
- 不要从静态对象中引用视图
- 避免将视图放进没有明确内存模式的收集中

## Marshmallow Update: Profile GPU Rendering

- Sync & Upload
> it takes to upload bitmap information to the GPU

## Cachematters for networking 
- DiskLruCache.java
- Tools: [Network Traffic Tool](https://developer.android.com/studio/profile/ddms.html#network?utm_campaign=android_series_#cachematters_for_networking_101315&utm_source=anddev&utm_medium=yt-annt) in Android Studio or [ARO tool](https://developer.att.com/application-resource-optimizer?utm_campaign=android_series_#cachematters_for_networking_101315&utm_source=anddev&utm_medium=yt-annt)

## Optimizing Network Request Frequencies

- Battery & Networking 
> Network 请求是最大的单一耗电王 
