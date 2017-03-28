# Something For Android Memory
> reference by [Android Performance Patterns](https://www.youtube.com/playlist?list=PLWz5rJ2EKKc9CBxr3BVjPTPoDPLdPIFCE)
> Knowledge for Android Render Performance , Memory and Garbage Collection

## Understanding Android Threading
寻找合适的帮手帮助更新UIThread
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


## Do not leak views

- 不要引用Async Callbacks(异步回调函数)内部的视图
- 不要从静态对象中引用视图
- 避免将视图放进没有明确内存模式的收集中
