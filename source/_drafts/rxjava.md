# 操作符
## compose
使用转入的transformer将原来的observable转换成新的observable

## first
发送第一个满足条件的数据,然后则结束订阅

## isEmpty
返回当前的observable里有没有数据,true则没有数据

## combineLatest和zip
zip是只有当2个observable都发新数据时,才会合并;而combineLatest是只要有1个observable有新数据,就会合并.

## TakeUntil (Observable other)
当other发射数据时,本Observable就会取消再发送数据,解除订阅

## TakeUtil (Predicate) 
当检查到Predicate返回true时,本Observable就会取消再发送数据,解除订阅

## repeatWhen
当参数里的Function返回Observable时,则会重复发送当前的Observable,可实现定时执行任务.

## distinct
去掉重复的数据,`distinct(Func1)`则可以定义一个key作为判断重复的字段

# Subject
把一个数据比较容易的转换成rx来使用,被观察和观察者都是它
## AsyncSubject
无论订阅的时候AsyncSubject是否completed,永远只收到最后一个值.
注意发送数据调用`onNext()`之后,记得要`onComplete()`,不然observer不会监听到数据的.

## BehaviorSubject
订阅时会收到最近发送的一个值

## PublishSubject
从哪里订阅就从哪里收到数据

## ReplaySubject
无论何时订阅,所有历史数据都收到

# Relay
跟Subject相似的功能,不同处在于,Subject当观察者出现异常时(收到`onComplete`,`onError`,会导致被观察者也停止发送数据,Relay则不受影响.
Relay有BehaviorRelay/PublishRelay/ReplayRelay三种,跟Subject下名称相同的功能是一样的.


# 想法
## 处理多个请求处理
rx.xxx(Event1, Event2, Consumer);

## flatmap递归
flatmap里面递归调用会非常有趣,例如可以递归调用`directory.listFiles()`不断获取文件列表

## Observable不返回null
在当不想返回数据到队列里的时候,不能直接返回null,因为reactive是没有null的,null将会导致链式调用无法进行下去,可以使用Observable.empty() 直接onComplete(),Observable.never()则不调用任何回调.

## 链式调用

# 结束

