#RxJava
______


[TOC]

## Observables
> 在ReactiveX当中，很多指令可能是并行执行的，之后他们的执行结果才会被观察者捕获，顺序是不确定的。未达到这个目的，你定义一种获取和变换数据的机制，而不是调用一个方法。这种机制下，存在一个可观察对象(Observable)，观察者(Observer)订阅(Subscribe)它。

### 回调函数
下面是一个比较完整的例子
```
// 三个回调函数
def myOnNext = { item -> /* do something useful with item */ };
def myError = { throwable -> /* react sesibly to a failed call */};
def myComplete = { /* clean up after the final response */ };

// 一个观察者对象
def myObservable = someMethod(itesParameters);
// 调用
myObservable.subscribe(myOnNext,myError,myComplete);

```

### 取消订阅

```
// 获得取消订阅的对象
Subscription subscription = Observable.just()               .subscribe();

// Observable会停止发送新的数据项
subscription.unsubscribe();

```

### Observable的"冷"与"热"
> 根据Observable的实现会决定，Observable是何时开始发射数据
> 一个"热"Observable可能一创建完就发射数据
> 一个"冷"Observable可能会一直等待，知道有观察者订阅它，才开始发射数据

### 精髓- - -操作符
> 对于ReactiveX而言，Observable和Observer只是个开始
> ReactiveX真正强大的地方是在于操作符的组合。

下面是常用的操作符列表：
- **创建操作** Create, Defer, Empty/Never/Throw, From, Interval, Just, Range, Repeat, Start, Timer
- **变换操作** Buffer, FlatMap, GroupBy, Map, Scan和Window
- **过滤操作** Debounce, Distinct, ElementAt, Filter, First, IgnoreElements, Last, Sample, Skip, SkipLast, Take, TakeLast
- **组合操作** And/Then/When, CombineLatest, Join, Merge, StartWith, Switch, Zip
- **错误处理** Catch和Retry
- **辅助操作** Delay, Do, Materialize/Dematerialize, ObserveOn, Serialize, Subscribe, SubscribeOn, TimeInterval, Timeout, Timestamp, Using
- **条件和布尔操作** All, Amb, Contains, DefaultIfEmpty, SequenceEqual, SkipUntil, SkipWhile, TakeUntil, TakeWhile
- **算术和集合操作** Average, Concat, Count, Max, Min, Reduce, Sum
- **转换操作** To
- **连接操作** Connect, Publish, RefCount, Replay
- **反压操作**用于增加特殊的流程控制策略的操作符


## 一些常用的操作符

### 创建操作

#### from
>用于创建Observable的集合
```
//Converts a Future into an Observable.
static <T> Observable<T>	from(java.util.concurrent.Future<? extends T> future)
```

```
//Converts a Future into an Observable, with a timeout on the Future.
static <T> Observable<T>	from(java.util.concurrent.Future<? extends T> future, long timeout, java.util.concurrent.TimeUnit unit)
```
```
//Converts a Future, operating on a specified Scheduler, into an Observable.
static <T> Observable<T>	from(java.util.concurrent.Future<? extends T> future, Scheduler scheduler)
```
```
//Converts an Iterable sequence into an Observable that emits the items in the sequence.
static <T> Observable<T>	from(java.lang.Iterable<? extends T> iterable)
```
```
//Converts an Array into an Observable that emits the items in the Array.
static <T> Observable<T>	from(T[] array)
```

#### timer
> 延迟一定时间进行某个操作

```
public static final Observable<java.lang.Long> timer(long delay,                            java.util.concurrent.TimeUnit unit)
```

#### interval
> 创建一个按固定时间间隔发射整数序列的Observable

```
public static final Observable<java.lang.Long> interval(long interval,
                                  java.util.concurrent.TimeUnit unit)
```

#### range
> 发射一个范围内的有序整数序列，它接受两个参数，一个是范围的起始值，一个是范围的数据的数目

方法
```
public static final Observable<java.lang.Integer> range(int start,int count)
```

#### defer
> 冷Observable的创建，Defer操作符会一直等待直到有观察者订阅它，然后它再使用Observable工厂方法生成一个Observable。
> 注意：Defer对每个观察者都会这么做，因此尽管每个订阅者都以为自己订阅的是同一个Observable，事实上每个订阅者获取的是他们自己的单独的数据序列



### 组合操作

#### merge
> 合并多个Observables的发射物

```
public static final <T> Observable<T> merge(Observable<? extends T> t1,
                      Observable<? extends T> t2)
```

#### zip
> 通过一个函数将多个Observables的发射物结合到一起，基于这

#### mergeDelayError
>将多个Observable进行合并转成一个新的，注意转换的类型问题

```
//注意传入参数的两个类型匹配都为T
public static final <T> Observable<T> mergeDelayError(Observable<? extends T> t1,
                                Observable<? extends T> t2)                              
```
个函数的结果为每个结合体发射单个数据项。

```
//将o1和o2，合并并进行zipFunction
public static final <T1,T2,R> Observable<R> zip(Observable<? extends T1> o1,
                          Observable<? extends T2> o2,
                          Func2<? super T1,? super T2,? extends R> zipFunction)
```
eg：
```
Observable<Integer> evens = Observable.just(2, 4, 6, 8, 10);
Observable<Integer> odds = Observable.just(1, 3, 5, 7, 9);

Observable
    .zip(evens, odds, (v1, v2) -> v1 + " + " + v2 + " is: " + (v1 + v2))
    .subscribe(System.out::println);
```
输出
```
2 + 1 is: 3
4 + 3 is: 7
6 + 5 is: 11
8 + 7 is: 15
10 + 9 is: 19
```

### 过滤操作
#### take
> 限制数量
```
public final Observable<T> take(int num)
```

#### Skip
> 抑制Observable发射前N项数据


#### Distinct
> 过滤操作，过滤掉重复的数据项
> 过滤的规则是：只允许，还没有发射过的数据项通过


### 变换操作

#### flatMap
> 将一个发射数据的Observable变换为多个不同类型的Observable，然后将他们发射的数据合并后放到一个单独的Observable

#### scan
> 连续的对数据序列的每一项应用一个函数，然后连续发射结果
> Scan操作符对原始的Observable所发射的第一个数据应用一个函数，然后将结果作为自己的第一项数据发射出去

方法定义
```
public final Observable<T> scan(Func2<T,T,T> accumulator)
//衍生方法
public final <R> Observable<R> scan(R initialValue,
                     Func2<R,? super T,R> accumulator)
```

例子
```
 Observable.just("")
                .scan((s, s2) -> s + s2)
                .subscribe(System.out::print);
```

#### groupBy
> 将一个Observable分拆为一些Observables集合，它们中的每一个发射原始Observable的一个子序列

```
//注意返回参数的类型为GroupedObservable
public final <K> Observable<GroupedObservable<K,T>> groupBy(Func1<? super T,? extends K> keySelector)
```





### 辅助操作
#### do  
> do是RxJava为后续几个衍生方法所定制的源

#### doOnEach
> Observable每发送一次数据都会调用一次回调，此回调可用一个Action的Notification的变体作为传参

```
// 注意传参
public final Observable<T> doOnEach(Action1<Notification<? super T>> onNotification)
//
public final Observable<T> doOnEach(Observer<? super T> observer)
```

#### doOnNext
> doOnEach的衍生项，不同于doOnEach，这个所传入的参数不需要接受一个Notification的参数
```
Observable.just("")
                .doOnNext(string -> {
                //每一次数据都会进行操作
                    if (string == null || string.isEmpty())
                        throw new NullPointerException();
                })
                .subscribe(next -> {
                }, Throwable::printStackTrace);
```

### 错误处理
#### Retry
> 如果原始的Observable遇到错误，重新订阅它期望它能正常终止

Retry操作符不会将原始Observable的onError通知传递给观察者，他会订阅这个Observable，再给它一次机会无错误地完成它的数据序列

- JavaDoc: retry() 继续订阅并发射原始的Observable
- JavaDoc: retry(long) 会最多重新订阅指定次数，如果次数超过，会把最新一个`onError`通知传递给它的观察者
- JavaDoc: retry(Func2) 作为参数的函数中有两个参数：重试次数和导致发射`onError`通知的`throwable`

#### Retrywhen
> 和`retry`类似，区别是，`retryWhen`将`onError`中的`Throwable`传递给一个函数，这个函数产生另一个Observable，`retryWhen`观察结果再决定是否需要重新订阅

例子
```
Observable.create((Subscriber<? super String> s) -> {
      System.out.println("subscribing");
      s.onError(new RuntimeException("always fails"));
  }).retryWhen(attempts -> {
      return attempts.zipWith(Observable.range(1, 3), (n, i) -> i).flatMap(i -> {
          System.out.println("delay retry by " + i + " second(s)");
          return Observable.timer(i, TimeUnit.SECONDS);
      });
  }).toBlocking().forEach(System.out::println);
```

### blocking
> 阻塞操作，一个继承于普通的Observable类的阻塞Observable

- forEach( ) — 对Observable发射的每一项数据调用一个方法，会阻塞直到Observable完成
- first( ) — 阻塞直到Observable发射了一个数据，然后返回第一项数据
- firstOrDefault( ) — 阻塞直到Observable发射了一个数据或者终止，返回第一项数据，或者返回默认值
- last( ) — 阻塞直到Observable终止，然后返回最后一项数据
- lastOrDefault( ) — 阻塞直到Observable终止，然后返回最后一项的数据，或者返回默认值
- mostRecent( ) — 返回一个总是返回Observable最近发射的数据的iterable
- next( ) — 返回一个Iterable，会阻塞直到Observable发射了另一个值，然后返回那个值
- latest( ) — 返回一个iterable，会阻塞直到或者除非Observable发射了一个
- iterable没有返回的值，然后返回这个值
- single( ) — 如果Observable终止时只发射了一个值，返回那个值，否则抛出异常
- singleOrDefault( ) — 如果Observable终止时只发射了一个值，返回那个值，否则否好默认值
- toFuture( ) — 将Observable转换为一个Future
- toIterable( ) — 将一个发射数据序列的Observable转换为一个Iterable
- getIterator( ) — 将一个发射数据序列的Observable转换为一个Iterator




