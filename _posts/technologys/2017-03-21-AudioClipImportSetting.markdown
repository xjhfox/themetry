---
layout: post
title:  "Audio Clip Import Settings"
date:   2017-03-16 19:05:13 +0000
category: tech
---
做的项目里才刚开始,几个音乐,就发现占用了100MB的内存....
查了下发现Audio Clip Import Settings 设置不好,会占用很多的内存.

网上找到一篇博客,相关部分翻译了一下.  [原文][how]

**Audio Clip Import Settings**

使用Unity的默认设置,在多余情况下都可以很好的工作.当然,默认设置不是最佳,但是你的
游戏能正常运行就OK了.但是不幸的是这个规则不适合音频剪辑的导入设置(Audio Clip Import Settings).

**Memory matters**
**内存问题**

![ok]({{ site.url }}/assets/audioClip.png)

所有的音频剪辑被导入的时候都默认的设置为"Decompress On Load"的加载方式和"Vorbis"的压缩格式.你要非常小心这个设置,
当它被用在你所有的音频剪辑上(Unity默认这么做!)将消耗你大量的游戏内存!
看见上面截图里的信息框了吗?原始文件大小35.9MB,导入后大小10.7MB.这意味着这个音频剪辑会增加你10MB的游戏(档案)大小,但是播放它却需要接近36MB的内存! 如果你做的是PC游戏的话,这听起来并不可怕,因为PC游戏一般都有8GB的内存,但是在移动设备上内存依然是受限的.

**When I should use specific Load Type?**
**什么时候使用特定格式?**

我们直说吧.每种加载方式和压缩格式都可以组合,所以我们要知道什么情况下应该选择哪一种.下面是3种加载方式:
1. Compressed In Memory - 音频剪辑会被压缩后存储在内存中,在播放的时候解压.不会在播放时占用更多内存.
2. Streaming - 音频剪辑会存储在设备的常驻记忆体(硬盘,U盘等),并在播放时用流加载.储存和播放都不需要内存(至少内存不重要)
3. Decompress On Load - 音频剪辑不压缩的保存在内存中,这个选项需要最多的内存但是播放的时候占用的CPU资源最少.
那么如何选择呢?这要看

**Music and/or Ambient Sounds**
**音乐还是环境音效**

音乐都是很长的音频剪辑,会消耗很多内存.当然我们不希望播放的时候解压它,于是有2个选择:

1. 用"Streaming"加载模式和压缩格式"Vorbis".这个组合使用最少的内存,但是需要一些CPU和磁盘I/O.
2. 用"Compressed In Memory"加载模式和压缩格式"Vorbis".和上面的唯一不同就是I/O消耗变为内存占用.
注意:你可以滑动Quality滑动条调整压缩后大小.**通常100%太高了,我一般调到70%左右**
**注意:如果你有超过2个这样设置的音频剪辑同时播放,将会严重消耗CPU资源**

**Sound Effect**
**音效**

音效通常是短或中等长度的音频剪辑.可能播放的很频繁,或很少. 有以下方式处理:
1. **频繁播放的,很短的音效** 使用"Decompress On Load"加载 和 "PCM" 或者 "ADPCM" 压缩格式. 
当使用"PCM"选项时,不需要解压,而且加载很短的音频剪辑非常的快. 
使用"ADPCM"时,需要解压,但是相比"Vorbis"要轻量的多.
2. **频繁播放,中等长度的音效** 使用"Compressed In Memory" 和 "ADPCM"压缩格式."ADPCM"压缩后大小比"PCM"压缩后小3.5倍.
而且不会消耗"Vorbis"那么多的CPU资源.
3. **很少播放的,很短的音效** 使用"Compressed In Memory" 和 "ADPCM"压缩格式,理由和第二点一样.
4. **很少播放的,中等长度的音效** 使用"Compressed In Memory" 和 "Vorbis"压缩格式,相对使用频率来说,用"ADPCM"压缩后储存的文件太大了,宁可增加点CPU消耗来解压.

**Summary**
**总结**

永远记得检查你的Audio Clip Import Settings. 这些都是沉默的效率杀手. 正确的设置他们,可以空出更多的资源给其他重要的部分.
记得也要看一下[官方文档][doc]


---
{% if site.intensedebate_comments %}
  {% include intensedebate-comments.html %}
{% endif %}

[how]: http://blog.theknightsofunity.com/wrong-import-settings-killing-unity-game-part-2/
[doc]: https://docs.unity3d.com/Manual/class-AudioClip.html
