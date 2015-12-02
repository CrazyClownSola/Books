#RxJava
______



[TOC]

##一些常用的方法集合


### from
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

### mergeDelayError
>将多个Observable进行合并转成一个新的，注意转换的类型问题

```
//注意传入参数的两个类型匹配都为T
public static final <T> Observable<T> mergeDelayError(Observable<? extends T> t1,
                                Observable<? extends T> t2)                              
```

### timer
> 延迟一定时间进行某个操作

```
public static final Observable<java.lang.Long> timer(long delay,
                               java.util.concurrent.TimeUnit unit)
```

### take
> 限制数量

```
public final Observable<T> take(int num)
```

### interval
> 创建一个按固定时间间隔发射整数序列的Observable

```
public static final Observable<java.lang.Long> interval(long interval,
                                  java.util.concurrent.TimeUnit unit)
```

### scan
> 连续地对数据序列的每一项应用一个函数，然后连续发射结果
```
public final Observable<T> scan(Func2<T,T,T> accumulator)
```

### groupBy
> 将一个Observable分拆为一些Observables集合，它们中的每一个发射原始Observable的一个子序列

```
//注意返回参数的类型为GroupedObservable
public final <K> Observable<GroupedObservable<K,T>> groupBy(Func1<? super T,? extends K> keySelector)
```

### merge
> 合并多个Observables的发射物

```
public static final <T> Observable<T> merge(Observable<? extends T> t1,
                      Observable<? extends T> t2)
```

### zip
> 通过一个函数将多个Observables的发射物结合到一起，基于这个函数的结果为每个结合体发射单个数据项。

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


### do  
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


### flatMap
> 将一个发射数据的Observable变换为多个不同类型的Observable，然后将他们发射的数据合并后放到一个单独的Observable

```

```
