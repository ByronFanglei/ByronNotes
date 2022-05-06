# ReactiveX JavaScript（RxJS）- Day 5

## 异步（asynchronous）与 串流（stream）

### 异步（asynchronous）

* 什么是同步：我们通常在阅读程式码时候是按照前后顺序阅读的，自然而然会认为代码是按照前后顺序执行的，也就是先发生的代码先处理，而这个按照前后发生顺序执行的行为我们就称为「同步」行为

* 什么是异步：在我们的需求中，有些不一定按照代码前后顺序的行为我们就称为「异步」行为

```typescript
// 先来说一下 js 中怎么实现异步 很简单 setTimeout 
setTimeout(() => {
	console.log(1)
}, 1000)

console.log(2)
// 2
// 1

```

* 上面的代码，如果按照同步的方式，那么我们只需要等待 1 秒即可拿到想要的数据，但是如果我们在请求 网络接口 的时候用了同步方式，偏偏这个时候服务器比较慢，那么我们可能需要很久才能拿到想要的数据，这显然是很不合理的

![](https://ithelp.ithome.com.tw/upload/images/20200920/20020617lDtJqaNtNu.jpg)

* 这种情况下「异步」的设计就变得非常重要，当程式并没有任何其他的运算，只是单纯的「等待」时，我们就把它丢到一个暂存区内，让画面可以继续处理其他的行为，直到等待到我们要的数据时，在拿回处理的所有权，继续后续的程式运算

![](https://ithelp.ithome.com.tw/upload/images/20200920/20020617xWqt5K7Qt7.jpg)

* 当然这仅仅只是一个观念而已，异步背后的原理其实还有很多的，当执行有一个顺序后，就需要搭配使用串流了。


### 串流（stream）

* 一般观看一部电影的话，电影本身资源相对是比较大的，如果我们在观看的时候需要把整个资源下载下来再去观看，那这个太影响观影体验了

![](https://ithelp.ithome.com.tw/upload/images/20200920/20020617nNVEgvoUfE.jpg)

* 假设我们把视频切成一段一段的，然后我们在快要观看到下一段时候再去加载下一段资源，应该是比较合理的

![](https://ithelp.ithome.com.tw/upload/images/20200920/20020617L7X58bhXXV.jpg)

* 这就是串流背后做的事情，将资料分成小小的片段，再「串」起来分段「流」向同一个地方，以上述的例子来说就是播放影片的逻辑，以下是 伪代码

```typescript
// 伪代码
class Video {
	// 影片资源
	videoSouce = [];

	// 下载资源
	private downloadVideo = (minute) => {
		if (this.videoSouce[minute]) {
			return Promise.resolve(this.videoSouce[minute]);
		} else {
			fetch("...").then((data) => {
				this.videoSouce[minute] = data;
				return data;
			});
		}
	};

	// 跳转播放
	jumpPlay = (minute) => {
		if (this.videoSouce[minute]) {
			this.play(minute);
		} else {
			this.downloadVideo(minute).then((data) => {
				this.play(data);
			});
		}
	};

	// 播放
	private play = (minute) => {
		this.downloadVideo(minute + 1);
	};
}

const video = new Video();
// 开始播放
video.jumpPlay(0);
// 跳转45min
video.jumpPlay(45);

```



### ReactiveX 与串流

* 在ReactiveX 的观念中，我们会将所有发生的事情都视为串流！

* 以网页为例子，滑鼠的点击事件可以视为一连串事件的串流，除非网页关闭，否则这个事件持续可能会发生。

![](https://ithelp.ithome.com.tw/upload/images/20200920/20020617sjpEjSyH8q.jpg)

* 当网络请求时，也是一种串流，只是这种串流事件只发生一次就会结束

![](https://ithelp.ithome.com.tw/upload/images/20200920/20020617lgmyJXGIUA.jpg)

* 所有的行为都可以看作串流，那么怎样处理串流就很重要的了，不然很容易出现 callback hell 的情况，ReactiveX 就是结合了多种程式设计的观念和技巧，漂亮的解决了这些问题，我们只需要先有一个观念，就是尽量以**串流的方式思考就好**

