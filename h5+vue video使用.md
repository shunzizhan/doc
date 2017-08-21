# h5+vue videoʹ��

@(video)[h5|vue]

### ����

����Ŀ������Ҫ����Ҫ����С��Ƶ�����ѡ��h5�е�video�����������ѡ�����vue����ˣ��򵥵��˽�ѧϰ��һ�£����һ�£����в���֮�����벻��������

### api
����ժ¼һЩapi�ķ�������
#### ����
- play()
- pause()
- load()
#### ����
- src�������� ��ָ����ҪǶ����Ƶ���ĵ���URL��
- poster�������� ������Ƶ�ܹ�����ʱ������ָ��������Ƶ��ͼ���URL��
- preload�������ԣ�preload������������ָ���Ƿ���ǰ���أ�������ǰ������Ƶ���ݣ����û�������Ӧ����ʾ��
 - preload���ԵĿ�ѡֵ��
 - none���ڲ�����Ƶ֮ǰ��������
 - metadata��ֻ������Ƶ��meta��Ϣ���֡�Meta��Ϣ���ռ��˹�����Ƶ�ĳ��ȡ���Ƶ����ʼ����ͼ����Ƶ�ĳ��ȵ���Ϣ
 - auto����������ǰ������Ƶ
 - autoplay��preload���Խ���Ч
- autoplay�������ԣ������Ƶ����ʹ�ã����ϲ�����Ƶ�������߼�����
- mediagroup�������ԣ�ʹ���video����audioԪ�ؼ��ϻ�
- loop�������ԣ�ѭ�����ţ������߼�����
- muted�������ԣ��ر���Ƶ������ͨ�����������������߼�����
- controls�������ԣ����ڱ�ʾ�ܹ�������Ƶ������ֹͣ���û����棬�����߼�����
- width�������ԣ���ʾ��Ƶ�ĺ��
- height�������ԣ���ʾ��Ƶ���ݸ�

#### �¼�
- play
- pause
- progress
- error

###��Ƶ��ʽҪ��

��һ��ʹ��h5��video���鿴��[h5��api](http://www.w3school.com.cn/tags/tag_video.asp)���ǲ��Ǿ���so easy��but����˵�̫�磬��ʱ�Լ��������һ��mp4��ʽ����Ƶ����ֵ��`video`��`src`��ǩ��������ҳ�����ܵ�ʱ��ͻȻ����ֻ����Ƶ��û����Ƶչʾ����`��˼����jie`������£�ֻ�ùԹԵĶ��[֪��](https://www.zhihu.com/question/31883236)
- ��Ƶ�ı����ʽ���ԣ� MP4��4�ֱ��룬MPEG4(DivX)��MPEG4(Xvid)��AVC(H264)�� HEVC(H265)���ù���ת����H264����Ϳ�����ҳ����������
- ���벻һ��������Ҫת��

ok��ԭ���Ǹ�ʽ������Ҫ���Ǿ��з����ˣ�Ѱ��h265ת�빤�����κΰ����ˣ�ȴ��֪��ôʹ�ã��͹ԹԵ�������һ����Ƶ�ˣ��ṩһ��[���ߵ���Ƶ](http://babylife.qiniudn.com/FtRVyPQHHocjVYjeJSrcwDkApTLQ)(����Ƶ��Դ��������Դ�������������ʹ�ã�����������)

### vueû�������ܷ��Զ�����
����api��ָʾ��video��ǩ��`autoplay`�������ó�`autoplay`�Ϳ����Զ����ţ��о��ǲ��Ǻܼ򵥣���û�У�vue��`v-bind`һ�²���ok�ˣ���û�кܸ��ˣ�Ȼ�������������ӣ����������ӣ����������ӡ���Ȼ�������ҵ����Ʋ��԰ɡ�
ԭ���Ĵ������£�
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
����һ�Ȼ����Լ��ǲ���������������⣬���Ǿ���û���ҵ������������󣬸о�����������·�Ӱɣ�����������h5��`video`�Դ���һЩ������
[js��ô�ж�h5 video�Ƿ����˲��Ű�ť](https://segmentfault.com/q/1010000006072618)
[HTML5 video Ԫ�ؼ���ȡ��Ƶ�����¼�](http://yijiebuyi.com/blog/154df41ba6e6415cc2d427b1940b5334.html)
ok�����ǿ��Կ���`video.play()`������video��ʼ���ţ��Ǿ���ô�ɰɣ��޸�������`playClick�¼�`
```javaScript
	playClick(){
		this._dom = document.getElementById('myvideo');
		this.isPlay = !this.isPlay;
		this._dom.play();
	},
```
ok����Ƶ�Ϳ���ͨ�������Ȼ��ʼ������



### ��ο�����Ƶ�������ر�
��ʵ����������Ѿ�ʵ���ˣ����Ǳ��˻����е��Ի󣬰���`�ٷ�api`��ָʾ����vue��ͨ���������ԣ��Ϳ����ˣ�����autoplay��ô�Ͳ����أ������Կ��ۣ�֪����С���������Խ���һ�£�лл

### ����ж���Ƶ�Ѿ��������
���Ǹղſ���`video`��һ������`paused`�����жϵ�ǰ��Ƶ�Ƿ��ڲ�����״̬����ô��������ϣ�Ҳ�ǿ���ͨ��`paused`�������жϵģ��������˼·���ײ���ok�ģ���ô����ж��Ƿ��Ѿ�������ϣ��о��Ͳ���ô�����ˣ�����ֻ��Ҫһ����ʱ�����ɣ��������£�
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
����һ�£��о�����Ū����ʱ�����о���low,`video`�и��¼������Լ����Ƶ�Ƿ񲥷���ϣ�
``` javaScript
	let _this = this;
		_this._dom = document.getElementById('myvideo');
		_this._dom.addEventListener('ended', function (e) {
			// ���Ž���ʱ����
			if(_this.count>0){
				_this.skip(); //��������ڶ�����Ƶ������ҳ��
			}else{
				_this.count += 1;
				_this._dom.removeAttribute('autoplay');
				_this.videoSrc = _this.testObj[1].videoSrc;
				_this.videoImg = _this.testObj[1].videoImg;
				_this.isPlay = false;
			}
			
		})
```

### �ܽ�
ֻҪ˼·�����£��취�ܱ������

### �ο�����
- [video api](http://www.w3school.com.cn/tags/tag_video.asp)
- [H5��video��ǩ�����ԺͲ����¼�](http://blog.csdn.net/seven0404/article/details/51899491)
- [ʹ��html��<video>��Ϊʲô��������Ƶ��](https://www.zhihu.com/question/31883236)
- [js��ô�ж�h5 video�Ƿ����˲��Ű�ť](https://segmentfault.com/q/1010000006072618)
- [HTML5 video Ԫ�ؼ���ȡ��Ƶ�����¼�](http://yijiebuyi.com/blog/154df41ba6e6415cc2d427b1940b5334.html)