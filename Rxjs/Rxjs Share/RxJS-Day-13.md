# ReactiveX JavaScript（RxJS）- Day 13

## 认识Cold Observable 与Hot Observable

### Cold Observable

* Observable 产生的数据流就属于 Cold Observable

可以看下之前是怎么建立一个 Observable 的

对于每次订阅来说，都是一次新的资料流产生；我们可以把整个资料流当作是一条**水管管线设计图**，每次订阅时，都是依照这条管线组装出新的水管线路，然后装水打开开关让水流出，这样的好处是**每次订阅都是新资料流的所以不会互相影响**。

![https://ithelp.ithome.com.tw/upload/images/20201018/20020617kfBEbRr5Co.jpg](https://ithelp.ithome.com.tw/upload/images/20201018/20020617kfBEbRr5Co.jpg)

这种每次订阅都重新开始的流程，称为「**Cold Observable**」。Cold 通常代表被用到时才启动的行为，在这里也是如此，说明了这个Observable 被订阅时才会开始启动新的资料流。

以一般的观察者模式的行为来说，每次事件的发生都是「推送」给观察者(Observer) 的；而Cold Observable 每次订阅后就只会有一个观察者，下一个观察者要进行订阅时会是一次新的资料流程，因此Cold Observable 与observer 是「**一对一**」的关系，这种只会推给唯一一个观察者的方式也称为「**unicast**」



### Hot Observable

* Subject 系列产生出来的数据流都是属于Hot Observable

一样当作水管管线来看的话，Subject就是一条随时可能有资料流过的线路，每次订阅都只是等待这条水管线路传送资料过来而已，而这样的好处是更有弹性，因为不用事先就把所有流程准备好，可以随时依照不同情境在程式内让新的事件发生(只要呼叫 next() 就好)，所有的观察者都会及时收到这份资料。

![https://ithelp.ithome.com.tw/upload/images/20200928/20020617SSEXsfxQFy.jpg](https://ithelp.ithome.com.tw/upload/images/20200928/20020617SSEXsfxQFy.jpg)

这种资料流已经开始，随时订阅就是等待最新资料的流程，就称为「Hot Observerable」。Hot 本身就有随时准备好的意思，因此每次订阅时不用从头来过，只关注最新事件即可。

由于只会有一个Observable，在每次事件发生时都会推送给所有的observer，因此Hot Observable 与Observer 的关系是「一对多」的关系，又称为「multicast」。


* 严格来数是没办法将Cold Observable 转成Hot Observable ，因为Cold Observable 一定要有个「启动」的动作才会开始资料流，而Hot Observable 在被建立同时就是启动的状态了，但是我们之后可以用一些 operators 来进行转换比如 multicast / publish / refCount / share / shareReplay 这些 operators，当使用这些这样 operators后会有一个 订阅的操作， 所以也叫做 **Warm Observable**




## 总结

* Cold Observable：在每次订阅时，完整的资料流会重新产生；资料流与订阅者是一对一的关系。

* Hot Observable：在订阅前资料流已开始，每个订阅者订阅的都是同一个资料流；资料流与订阅者是一对多的关系。

* Warm Observable：在第一次订阅开始前不会启动资料流，直到第一次订阅发生启动，所有观察者都是订阅同一个资料流；资料流与订阅者是一对多关系。