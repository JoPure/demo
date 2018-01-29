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
      
