# jquery&zepto异同，事件代理demo

面tgideas时问到了这两个问题，说实话平时经常用，但真正讲点什么原理的时候一点都不知道，也答不上来个所以然
在网上查了下自己也做了一些笔记，搞了下两个小demo，日后再慢慢更详细的贴上更多例子:blush:

### 一、jquery&zepto异同
#### 同

    Zepto最初是为移动端开发的库，是jQuery的轻量级替代品，因为它的API和jQuery相似，而文件更小。
    Zepto最大的优势是它的文件大小，只有8k多，是目前功能完备的库中最小的一个，尽管不大，Zepto所提供的工具足以满足开发程序的需要。
    大多数在jQuery中·常用的API和方法Zepto都有，Zepto中还有一些jQuery中没有的。
    另外，因为Zepto的API大部分都能和jQuery兼容，所以用起来极其容易，如果熟悉jQuery，就能很容易掌握Zepto。
    你可用同样的方式重用jQuery中的很多方法，也可以方面地把方法串在一起得到更简洁的代码，甚至不用看它的文档。

#### 异

    针对移动端，zepto有一些基本的触摸事件可以用来做触屏交互，例如touch，swipe事件，zetpo不支持ie浏览器的，
    是为了在降低文件尺寸而做的决定,因为不支持，便有建议在ie浏览器上用jqery作为后备库，
    这样能够保证在ie浏览器上也能实现开发者需要的功能，不过两者的api不一定完全兼容，还有一些方法是不同的,根据以往项目经验，我们逐一展示下


#### 1.获取dom的高宽度上,width();height();
 
 
 `用zetpo会把border也计算上`,
 jquery则是正常输入width&height的设定值

![][demo2]

#### 2、获取dom的offset();
   如图  zetpo 获取
   
![][demo3]
     我们再试试jquery的 返回的是
     
![][demo3]

zetpo获取dom.offset方法匹配元素在文档位置时，会返回height，left，top，width，
    jquery只有left，top

     
####  3、zetpo的each方法只能遍历数组，不能遍历json对象，
####  4、zetpo对下拉选框以及选项卡radio的不同
      Zepto在操作dom的selected和checked属性时尽量使用prop方法，在读取属性值的情况下优先于attr。
      Zepto获取select元素的选中option不能用类似jQuery的方法$('option[selected]'),因为selected属性不是css的标准属性。
      应该使用$('option').not(function(){ return !this.selected })。
      
  
jquery则正常使用  下文是获取select下拉框选中的文本

![][demo3]


####  5、Zetpo不支持的选择器
- 基本伪类 
:first , 
:not(selector) ,
:even, 
:eq(index), 
:gt(index), 
:lang1.9+, 
:last, 
:lt(index), 
:header, 
:animated, 
:focus16.+ ,
:root1.9+, 
:target1.9+
- 内容伪类 :contains(text), :empty, :has(selector), :parent
- 可见性伪类 :hidden :visible;
- 属性选择器 [attribute!=value]
- 表单伪类 :input :text :password :radio :checkbox :submit :image :reset :button :file :hidden
- 表单属性伪类 :selected
      




### 事件委托
接下来我们说一下事件委托，也就是事件代理，
他的概念是，基于js的冒泡事件，可以以js事件委托的方式，在父元素上，给父元素的子元素绑定事件，同时减少事件绑定的复杂度，
通俗一点讲，使用事件委托能让我们避免对每个节点都添加监听器，相反，监听器可能在父元素上，父元素的监听器会分析子元素冒泡上来的事件，
找到是哪个子元素的事件，

那在这里我们首先讲讲 `冒泡 `

> 冒泡的意思是， `从事件最深的节点开始，然后逐步向上传播事件 `

        页面上有这么一个节点树，
        div>ul>li>a;
        比如给最里面的a加一个click点击事件，那么这个事件就会一层一层的往外执行，执行顺序a>li>ul>div，
        有这样一个机制，那么我们给最外面的div加点击事件，那么里面的ul，li，a做点击事件的时候，都会冒泡到最外层的div上，所以都会触发，
        这就是事件委托，委托它们父级代为执行事件。

这样可以 `减少事件绑定的复杂度,`
`特别是动态添加的子元素，采用事件委托，新元素也可以被监听到， `

例如ul 里li 的点击事件，比如需要对li 进行事件发生，那有100个li时，我们可能会用for 循环去遍历每个li，然后给他们添加事件，这样的话，
在javaScript中，添加到页面上的事件处理程序就会变得特别多，而性能优化其中之一就是减少dom操作，

#### 补充 
    在不断的进行重绘与dom的加载访问事件，会延长整个页面的交互时间，拖慢访问，占用内存，甚至会影响到页面整体的操作交互，
    这时候如果我们采用事件委托，直接对li 的父元素 ul 进行监听，并且当有新的li生成时，页面也只是对li进行重绘，此时，监听的dom元素依旧是父元素，

demo里有这么一段代码,这是一个ul li,10个节点,

这是最初我弄的，没遇到事件委托的原理前，连动态我都是这么搞的，后来才知道会很拖慢速度 并且，也不好维护:anguished:	


![][demo6]

```javascript

    //点击选中图标进行竞猜   直接li点击的
        $("#parent-list li").on("click", function () {
         if (!$('.menu-bar li.active').hasClass("disClick")) {
             if (pg_config.data.count < 5 && !$(this).hasClass("active")) {
                 pg_config.data.count++;
                 $(this).addClass("active");
             } else if ($(this).hasClass("active")) {
                 $(this).removeClass("active");
                 pg_config.data.count--;
             }
         }
     });



```

重构之后是这样，我们可以看到不同是，没有直接对li进行监听，而是ul父元素进行监听，

```javascript
    //如果节点是li 则进行接下来的操作
    if (e.target.parentNode && e.target.parentNode.nodeName == "LI") {
    }

```

```javascript

 $("#parent-list").on("click", function (e) {
        if (e.target.parentNode && e.target.parentNode.nodeName == "LI") {
            if (!$('.menu-bar li.active').hasClass("disClick")) {
                if (pg_config.data.count < 5 && !$(this).hasClass("active")) {
                    pg_config.data.count++;
                    $(e.target.parentNode).addClass("active");
                } else if ($(this).hasClass("active")) {
                    $(e.target.parentNode).removeClass("active");
                    pg_config.data.count--;
                }
            }
        }
    });


```

这就是事件委托最普遍的demo啦，记录下来，日后有需要再了解的我们再参考下
