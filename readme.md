## 记录一下对rem的理解

**注：这是我之前已经发布过的一篇文章，只是不方便查看，今天把它整理出来单独作一个知识点方便后面学习查看！**

### 简单阐述一下什么是rem！

* 其实说的通白一点，像类似rem，em等这些单位，你完全可以把它简单理解为跟自己熟悉的单位px是一样的东西，因为他们都是一种衡量长度的单位而已。只是不同的地方它们是相对于一个已定单位来说的，比如说，1em就代表的是一个font-size的大小，不过注意，em是相对于它的先辈节点设置的font-size或者当前元素的font-size来说的，em总是一个遵守一个最近原则，也就是说，em首先会查找当前元素是否设置font-size大小，如果没有，再向上一级查找，以此类推，直到找到一个显示设置font-size或者继承的font-size大小的元素，从而得到一个em的长度！好了，其实rem跟em是一样的，唯一的不同点，它跟em计算的时候相对的节点不同，rem无论什么时候都是相对于页面最先设置那个节点来计算rem值的，在html页面，你可以理解那个节点就是html节点！



### rem的好处

* 1、在html5开发种，使用rem作为单位的好处就是能保证在各个分辨率情况下的，页面都会是一个相似或者说相同的布局，从某种意义上来说，rem布局跟百分比布局是差不多的概念。打个比方，如果html的页面布局宽度为320px，如果此时给html设置font-size为100px，那么给目标节点的宽度设置为1.6rem，也就是说宽度换算出来就是宽度就是160px，换句话说，此时给这个div设置宽度为1.6rem，其实就相当于给它设置宽度为50%。所以说使用rem布局来实现响应式布局也是很不错的！

* 2、之前在使用em布局的时候，你可能会觉得em找到那个设置了font-size的节点是个比较无趣和痛苦的过程，因为总是相对的不一样，所以rem完全可以解决你这个顾虑，因为无论什么时候，它都只是相对于根节点。


### rem的不好地方

* 1、按我目前的理解就是，与em布局总是查找那个相对的节点的过程无聊来说，rem布局则是集计算与测量的一个复杂过程，因为你要在每个分辨率下准确计算当前html的font-size大小，还要把dom的尺寸换算成rem值，这个过程都是要从设计稿中查看与测量得来的！




### rem实现方式

* 1、css方式

使用css方式主要过程就是通过媒体查询来是设置当前html的font-size大小，比如下面这样一段代码

```
html{
	font-size:10px;
}
@media (min-width:321px) and (max-width:375px){
	html{
		font-size:11px;
	}
}
@media (min-width:376px) and (max-width:414px){
	html{
		font-size:13px
	}
}
@media (min-width:415px) and (max-width:639px){
	html{
		font-size:15px;
	}
}
@media (min-width:640px) and (max-width:719px){
	html{
		font-size:20px;
	}
}
 //后面代码略
```

 首先说一下这种方法就是不好的地方就是写了很多的媒体查询，比较痛苦，这个问题可能是使用@media query布局的通病了，最重要的是这种方式定义的是一种区段的方式，所以不连续，会造成某些时候尺寸并不是我们想要的效果。而且在实际生产中，可能还有很多种不同尺寸的设备，所以上面的media query可能还匹配不到，所以这是这种方式的短板。我也不建议使用这种方式！



 * 2、js方式
 
 利用这种方式的关键是我们要记住一个公式：

 ```
 		1、dom节点的rem值 = dom尺寸 / html的font-size
 ```

 反过来求当前html的font-size大小就是用下面公式：

 ```
 		2、html的font-size = dom尺寸 / dom节点的rem值
 		注意这里的dom尺寸就是document.documentElement.clientWidth!
 ```
 实际中就是反复套用这两个公式，就能算出dom的rem值，以及当前的html的font-size！


 比如：


	当我们得到一张从设计师那里发来的设计图之后，假设设计图是基于iphone4的，那我们知道iphone4竖直摆放时的水平尺寸是320px，所以设计图宽度应该来说水平宽度为640px，至于为什么是640px，可以说目前业界web app页面开发的一种通用流程了，相关概念需要设计iphone 的retina概念，但是这里我希望做这样的理解即可：设计图应按照2倍尺寸来做就对了！相关概念可以查看retina和高清分辨率等资料！




	ok我们把当前的html的font-size大小就作100px算，为什么要用100px，无非只是为了好计算而已，并没有什么特别原因，你也可以完全使用150px等其他尺寸，最后换算时候只要用上面两个公式换算对应的尺寸就没有问题。



	好了，现在html的font-size为100px，那么页面的宽度，也就是body的宽度用上面第一个公式是不是可以算出为6.4rem，其他的dom节点一样的按这个公式算出，比如在设计图上看到一个div的宽度为50px，那么的宽度就可以设置为0.5rem，就是这么简单。



	但是上面的html的font-size大小是根据页面宽度为640px设计的，但是当我们的项目实际上线时候，可能遇到的客户端就有很多种不同的尺寸了，那么怎么动态的计算html的font-size大小呢？这下就要用到我们的第一个公式了，比如页面在iphone4中被浏览时，此时的页面宽度应该为320px,所以根据第一个公式，当前的font-size就是320/6.4等于50px了！所以当页面被加载时候你需要执行下面一个函数！


```

	(function(){
		var html = document.documentElement;
		var _width = html.offsetWidth;
		if(_width>640){
			html.style.width = 640+'px';
		};
		html.style.fontSize = html.clientWidth / 6.4 + 'px';
	})();
```

	到此，基本上就算rem布局大工工程了，现在你可以测试的页面，可以看见会有一个比较好的缩放效果。
