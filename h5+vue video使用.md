# h5+vue video使用

@(video)[h5|vue]

### 背景

因项目需求需要，需要播放小视频，因此选择h5中的video，但技术框架选择的是vue，因此，简单的了解学习了一下，随记一下，如有不到之处，请不吝修正。

### api
下面摘录一些api的方法属性
#### 方法
- play()
- pause()
- load()
#### 属性
- src内容属性 ：指定所要嵌入视频、文档的URL。
- poster内容属性 ：当视频能够播放时，用于指定代表视频的图像的URL。
- preload内容属性：preload内容属性用于指定是否提前下载，怎样提前下载视频数据，给用户代理相应的提示。
 - preload属性的可选值：
 - none：在播放视频之前不做下载
 - metadata：只下载视频的meta信息部分。Meta信息中收集了关于视频的长度、视频的起始祯的图像、视频的长度等信息
 - auto：尽可能提前下载视频
 - autoplay：preload属性将无效
- autoplay内容属性：如果视频可以使用，马上播放视频，属于逻辑属性
- mediagroup内容属性：使多个video或者audio元素集合化
- loop内容属性：循环播放，属于逻辑属性
- muted内容属性：关闭视频的声音通道（静音），属于逻辑属性
- controls内容属性：用于表示能够操作视频播放与停止的用户界面，属于逻辑属性
- width内容属性：表示视频的横宽。
- height内容属性：表示视频的纵高

#### 事件
- play
- pause
- progress
- error

###视频格式要求

第一次使用h5的video，查看了[h5的api](http://www.w3school.com.cn/tags/tag_video.asp)，是不是觉得so easy，but别高兴的太早，当时自己随便找了一个mp4格式的视频，赋值给`video`的`src`标签，但是在页面上跑的时候，突然发现只有音频，没有视频展示，在`百思不得jie`的情况下，只好乖乖的度娘，[知乎](https://www.zhihu.com/question/31883236)
- 视频的编码格式不对， MP4有4种编码，MPEG4(DivX)，MPEG4(Xvid)，AVC(H264)， HEVC(H265)，用工具转换成H264编码就可以网页正常播放了
- 编码不一样。你需要转码

ok，原来是格式有特殊要求，那就有方案了，寻找h265转码工作，奈何按照了，却不知怎么使用，就乖乖的重新找一个视频了，提供一个[在线的视频](http://babylife.qiniudn.com/FtRVyPQHHocjVYjeJSrcwDkApTLQ)(此视频来源于网络资源，如果不能正常使用，请自行重找)

### vue没法控制能否自动播放
按照api中指示，video标签的`autoplay`属性设置成`autoplay`就可以自动播放，感觉是不是很简单，有没有，vue中`v-bind`一下不就ok了，有没有很高兴，然而并不是这样子，不是这样子，不是这样子【当然或许是我的姿势不对吧】
原来的代码如下：
```vbscript-html
<template>
	<div>
		<video id="myvideo" :src="videoSrc" :poster="videoImg" :muted="muteStatus" :autoplay="playStatus" height="414" width="720">
			your browser does not support the video tag
		</video>
		<span class="ico ico-sound" :class="{ active: isMute }" v-on:click="closeSoundClick()"></span>
		<span class="ico ico-skip"></span>
		<span class="ico ico-video" :class="{ hide: isPlay }" v-on:click="playClick()"></span>
	</div>
</template>
```
```javaScript
	data(){
		return {
			_dom:"",
			videoSrc:'http://babylife.qiniudn.com/FtRVyPQHHocjVYjeJSrcwDkApTLQ',
			videoImg:'http://static.fdc.com.cn/avatar/usercenter/5996999fa093c04d4b4dbaf1_162.jpg',
			playStatus:'',
			muteStatus:'',
			isMute:true,
			isPlay:false
		}
	},
	methods:{
		playClick(){
			this.isPlay = !this.isPlay;
			this.playStatus= 'autoplay';
		},
		closeSoundClick(){
			this.isMute = !this.isMute;
			if(this.isMute){
				this.muteStatus = '';
			}else{
				this.muteStatus = 'muted';
			}
		}
	}
```
心理一度怀疑自己是不是哪里出现了问题，但是就是没有找到问题在哪里，最后，感觉还是重新找路子吧，咱们来找找h5中`video`自带的一些方法吧
[js怎么判断h5 video是否点击了播放按钮](https://segmentfault.com/q/1010000006072618)
[HTML5 video 元素及获取视频播放事件](http://yijiebuyi.com/blog/154df41ba6e6415cc2d427b1940b5334.html)
ok，我们可以看到`video.play()`可以让video开始播放，那就这么干吧，修改上述的`playClick事件`
```javaScript
	playClick(){
		this._dom = document.getElementById('myvideo');
		this.isPlay = !this.isPlay;
		this._dom.play();
	},
```
ok，视频就可以通过点击，然后开始播放了



### 如何控制视频的声音关闭
其实这个，基本已经实现了，但是本人还是有点迷惑，按照`官方api`的指示，在vue中通过控制属性，就可以了，但是autoplay怎么就不行呢，真是脑壳疼，知道的小主，请留言解释一下，谢谢

### 如何判断视频已经播放完毕
我们刚才看到`video`的一个属性`paused`可以判断当前视频是否处于播放中状态，那么，播放完毕，也是可以通过`paused`属性来判断的，带着这个思路，亲测是ok的，那么这个判断是否已经播放完毕，感觉就不那么困难了，我们只需要一个定时器即可，代码如下：
``` javaScript
showOtherVideo(){
	let _this = this;
	setTimeout(function(){
		_this.flag = _this._dom.paused;
		if(!_this.flag){
			_this.showOtherVideo();
			console.log(_this.flag);
		}
				
	},1000)
}
```
修正一下，感觉这样弄个定时器，感觉很low,`video`有个事件，可以监测视频是否播放完毕，
``` javaScript
	let _this = this;
		_this._dom = document.getElementById('myvideo');
		_this._dom.addEventListener('ended', function (e) {
			// 播放结束时触发
			if(_this.count>0){
				_this.skip(); //当播放完第二个视频进入主页面
			}else{
				_this.count += 1;
				_this._dom.removeAttribute('autoplay');
				_this.videoSrc = _this.testObj[1].videoSrc;
				_this.videoImg = _this.testObj[1].videoImg;
				_this.isPlay = false;
			}
			
		})
```

### 总结
只要思路不滑坡，办法总比问题多

### 参考资料
- [video api](http://www.w3school.com.cn/tags/tag_video.asp)
- [H5的video标签的属性和播放事件](http://blog.csdn.net/seven0404/article/details/51899491)
- [使用html中<video>，为什么看不了视频？](https://www.zhihu.com/question/31883236)
- [js怎么判断h5 video是否点击了播放按钮](https://segmentfault.com/q/1010000006072618)
- [HTML5 video 元素及获取视频播放事件](http://yijiebuyi.com/blog/154df41ba6e6415cc2d427b1940b5334.html)