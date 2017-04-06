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

## Effective Prefetching
- GCMNetworkManager
The GCM enables client apps to register services that perform(实现) network-oriented(网络化) tasks based on specified criteria(指定标准) such as :
    - Schedule one-off vs. periodic tasks.(安排一次周期性任务)
    - Automatic back-off and failure retry.(自动重连)
    - Awareness of network activity.(意识到网络请求)
    - Awareness of device charging state.(意识到设备进入充电状态)

## Adapting To Latency
> 处理你的app 适应网络环境的变更
- Gather information 做网络算法获取到网络情况
- Make adjustments 

## Minimizing Asset Payload
> 可以利用AT&T的RBO工具

- PNG is not Enough 
> if you dont use alpha ,dont use png
- Array-of-Structures(利用二进制序列化格式) Protocol Buffers、Nano-Proto-BUffers、FlatBuffers 进行数据传参 
> Https 对于序列化参数采用的压缩算法是 GZIP
- Struct-Of-Arrays(把数据构造转成对数据的转置)
> 通过数据结构的重新序列化，可以使得相同的数据进行简单排列
- Serializable  是一种非常不好的实现，这种方式会占用大量内存，同时产生编码负担，可以采用GSON 进行替代，但是GSON的最大缺点是产生的json文件中会有很多冗余的语法存在
- Protocol Buffers 
> google提供的库，既紧凑又灵活，但是缺点是依赖于 Java安装这对于移动开发不是很理想
- Nano-Proto-Buffers
> 是为android专门优化过的Protocol Buffers 
- FlatBuffers
> 这个格式是有google的一个游戏团队开发的，非常强调性能，几乎在解码和编码的时候不占用任何时间

## Do Something with Overdraw

- Cliprect
- QuickReject

## Service Performance Patterns
> Service 是指Activity在自身生命周期之外，仍旧需要指示操作系统运行长时间的任
通常可以通过下列很多方法去替代Service
  Async Event Functions
  - GCM
  - BroadcastReciever
  - LocalBroadcastReciever
  - WekefulBroadcastReciever
  - HandlerThreads
  - AsyncTaskLoaders
  - IntentService

## Fun with ArrayMaps
> 集合是对于性能优化一个最致命的点，但是HashMap在创建的时候会占用大量的内存去保证他自身数组的有效性，所以Android专门为移动开发创建了新的类ArrayMa
> ArrayMaps 对于集合数量不大(数量1000)的数据进行数据处理时的性能优化是HashMap的好几倍
还有一个很好的优势是
```
// ArrayMap
for(int i =0; i< map.size();i++){
    Object key = map.keyAt(i)
    Object val = map.valueAt(i)
    ...
}

```
## FusedLocationProviderApi
> google 提供的Location Api接口

## Dont leak View
- Use [Hierarchy Viewer](https://developer.android.com/studio/profile/hierarchy-viewer.html#start)
- Use [Systrace](https://developer.android.com/studio/profile/systrace.html#app-trace)
- Use Update GPU
