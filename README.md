# killIE
> 早年学习笔记和一些浏览器兼容性处理方案

块级元素：`div p h1 hr ul li ol dl dt dd table tr form fieldset`

1. css各式排版技巧汇总
  
  #### 左固定，右自适应，兼容IE6+
  
  方法一：
    
        <div class="left"></div>
        <div class="right"></div>
        .left{float:left;width:200px;}
        .right{margin-left:200px;}  //不要浮动，设置左边距等于左边宽度
    
  > 在IE6中会产生3px间隙，使用`.left{_margin-right:-3px;}`即可修复

  方法二：使用定位

        .right{width:100%;position:relative;}
        .left{position:absolute;left:0;top:0;width:200px;}
        <div class="right">
	        <div class="left"></div>
        </div>
        
  #### 左右固定，中间自适应

  方法一：使用浮动，兼容IE6+

        .left{width:300px;height:800px;background:#99ffcc;float:left;_margin-right:-3px;}
        .right{width:500px;height:800px;background:#ffcc00;float:right;_margin-left:-3px;}
        .mid{height:800px;background:#660066;}
        <div class="left">1</div>
        <div class="right">3</div>
        <div class="mid">2</div>
        
  方法二：使用定位
  
  方法三：负margin
  
  #### 中间固定，左右自适应，兼容IE6+

  采用定位
  
        /*css*/
        *{padding:0;margin:0;}
        html,body{height:100%;}
        div{height:100%;}
        #mid{z-index:2;background-color:#eee;width:500px;margin-left:-250px;position:absolute;top:0;left:50%;}
        #left,#right{z-index:1;position:absolute;top:0;width:50%;}
        #left{left:0;}
        #left .container{margin-right:250px;background-color:#bbb;}
        #right{right:0;}
        #right .container{margin-left:250px;background-color:#bbb;}
        /*html*/
        <div id="mid">
            mid 宽度固定 : 500px
        </div>
        <div id="left">
            <div class="container">
                left 宽度自适应
            </div>
        </div>
        <div id="right">
            <div class="container">
                right 宽度自适应
            </div>
        </div>

2. css响应式设计

两种方法
  
> 注意头部加上<meta name="viewport" content="width=device-width,initial-scale=1.0" />让手持设备保持正常分辨率
> 同时禁止网页缩放<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no" />

外部引入 `<link rel="stylesheet" type="text/css" media="screen and (min-width:1024px)" href="../style1.css" />`

内联样式

        <style type="text/css">
          @media only screen and (min-width:1024px){
            .class:{
              width:980px;
            }
          }
        </style>
        
js事件(屏幕状态监控)

`window.orientation`

值：

  0 - 竖屏
	
  90 - 逆时针旋转横屏
	
  -90 - 顺时针旋转横屏
	
  180 - 竖屏，上下颠倒

手机横竖屏样式

        @media screen and (orientation:portrait){
          //竖屏样式
        }
        @media screen and (orientation:landscape){
          //横屏样式
        }
        
IE8以下不兼容，解决方法：

单独为IE设置css样式，并用以下方法引入

        <!--[if (lt IE 9) & (!IEMobile)]>
        <link rel="stylesheet" type="text/css" href="css/ie.css" />
        <![endif]-->

关键：

混合布局：流动布局（用百分比代替像素）+弹性布局（设置容器大小em）+固定布局

字体大小：单位em,rem(相对于根),px

图片处理：`img{max-width:100%;height:auto;}`

响应式布局小结：

* 最好使用图片代替背景图片

像logo等比较重要的图片还是使用<img>,背景图片的大小在不同分辨率不好控制

* 百分比的使用

看清楚哪个是直接父元素，用元素的实际像素/直接父元素的实际像素

手机版去除浮动，宽度设为100%

平板端按照电脑端等比例缩放

* 当直接放文字等的元素需要为其设置百分比宽度时，比如p,a,span等

可以设置{overflow:hidden;white-space:nowrap;text-overflow:ellipsis;}把超出部分用'...'代替，防止其破坏布局

3. css3常用

圆角边框border-radius:5px 5px 5px 5px;(左上 右上 右下 左下)

方框阴影box-shadow:5px 5px 5px 5px #000 inset;(水平阴影位置 竖直阴影位置 模糊距离（可选） 阴影尺寸（可选） 阴影颜色（可选） 内部阴影（可选）)

文本阴影text-shadow:5px 5px 5px #000;(水平阴影位置 垂直阴影位置 模糊距离（可选） 阴影颜色（可选）)

规定当文本溢出包含元素时发生的事情text-overflow:clip(裁剪文本)/ellipsis（用省略号代替）/string（使用给定的字符串来代表被修剪的文本）兼容IE6
	
  实现的必要条件{
	    
      1.overflow:hidden;
	    
      2.white-space:nowrap;
	    
      3.text-overflow:ellipsis;
	}
  
  > 如果不起作用(设置：word-wrap:normal)

  允许单词拆分到下一行word-wrap:break-word; 兼容IE6


2D转换（IE9+）

        transform:translate(50px,100px); //左移50px，下移100px
        transform:rotate(30deg);  //顺时针旋转30度
        transform:scale(2,4);  //宽度变为2倍，高度变成4倍
        transform:skew(30deg,20deg);  //围绕x轴改变30度，围绕Y轴改变20度
        
3D转换(IE10+)

        transform:rotate3d(x,y,z);  //3D转换
        transform:rotateX(120deg);  //围绕X轴旋转120度
        transform:rotateY(120deg);  //围绕Y轴旋转120度

过渡效果

        transition:width 2s ease 1s;  //需要过渡的属性 过渡需要时间 时间曲线 何时开始执行
  
  同时改变多个属性
           
        transition:width 2s,background 2s;
        //一般需要配合伪类如div:hover{width:100px;}使用
        
css3动画@keyframes规则

        //新建规则
        @keyframes name{
          from{background:red;}
          to{background:blue;}
          //0% {background:red;}
          //25%{background:blue;}
          //50%{background:yellow;}
          //100%{background:black;}
        }
        
        //应用规则（绑定动画）
        div{animation:name 5s}  //动画名称 持续时间（还可以规定何时开始、播放次数等）
        box-sizing:border-box理解：把padding,border等的大小也算入目标元素大小中，方便计算（免去减padding值等的烦恼） 兼容IE9+

        ::selection{color:#fff;background-color:#0ff;}  改变选中文本的状态
        IE,ff不支持，chrome,opera,safari支持

4. css兼容性汇总

继承父级属性inherit兼容IE9+,慎用

选择器部分：

  属性选择器

        [title]{width:300px;height:100px;}意为带有title属性的所有元素
        [title=hello]{width:300px;height:100px;}意为title属性为hello的所有元素
        input[type=text]{border:1px solid #9ff;}意为type属性为text的input元素
        //兼容IE7+，IE6不支持
        
  子元素选择器（只会选择到直接子元素）
  
        h1>strong{color:red;}
        <h1>This is <strong>very</strong> important!</h1> //very为红色
        <h1>This is <em><strong>very</strong></em> important!</h1> //very不是红色
        //IE6不兼容
        
  相邻兄弟选择器（只会选择紧接在另一元素后的元素，且二者有相同父元素）
  
        h1+p{margin-top:50px;}
        <h1>This is a heading.</h1>
        <p>This is paragraph.</p>  //marin-top为50px
        <p>This is paragraph.</p>  //不会为它设置margin-top
        //IE6不兼容
        
  伪类（用于向某些选择器添加特殊的效果）
  
        链接四种状态、:focus(input标签可用，表示选中时状态)、:first-child(向元素的第一个子元素添加样式)
        //兼容IE7+,超链接兼容IE6
        :last-child最好不用,不兼容IE8
        
  伪元素
  
        :first-letter(向文本的第一个字母添加特殊样式)、:first-line(向文本的首行添加特殊样式)、:before(在元素之前添加内容)、:after(在元素之后添加内容)
        //:first-letter和:first-line只能用于块级元素；如p:first-letter  兼容IE6+
        //:before、:after 如p:before{content:url(01.jpg);} 不兼容IE6


  边框部分：css3的border-radius(设置圆角)和box-shadow(设置阴影)支持IE9+

  文本部分：white-space处理空白
    值pre不会忽略空白符兼容IE8+/
    nowrap防止文本换行，除非使用<br>兼容IE6+/
    pre-wrap和pre-line兼容IE8+
    text-shadow文本阴影，兼容IE10+

  轮廓：outline元素外部轮廓
  
    可以设置元素的轮廓大小 颜色 线状   //规定!DOCTYPE后兼容IE8+
    *outline不占据空间

  框模型：
  
    IE6中3pxBUG描述：在IE6中，当一个浮动元素和一个未浮动元素相邻时，会产生3px的margin
    
    解决方案：css hack `_margin-left:-3px;`只能在IE6下识别

  img图片下接块级元素会出现间隙
  
    解决方案：为图片设置`display:block;`

  IE6种浮动元素双倍margin问题描述：在IE6中，当给一个浮动块级元素设置margin值时，会产生双倍
  
    解决方案：设置这个块级元素的`display:inline;`
    
  浮动元素margin-bottom失效
  
    描述：在IE6，7，8中，为浮动元素设置margin-bottom会失效，当下方也为浮动元素时有效
    
    解决方法：
    
        1.尽量避免为浮动元素设置margin-bottom，必要时用padding-bottom代替；
        加上{*padding-bottom:30px;}
        2.在浮动元素后添加<div class="clearfix></div>
        .clearfix{clear:both;}这样会添加多余代码

  IE6中一个容器中有一个浮动元素和一个不浮动元素，可能导致换行
  
    解决方案：把浮动元素放在未浮动元素前面

  定位问题：`position:fixed //兼容IE7+,IE6不支持`
  
    解决方案：通过js来实现IE6下的固定定位；
    
        <style type="text/css">
        *{margin:0;padding:0;}
        /*修复IE6抖动*/
        * html{_background:url(about:blank) fixed;}
        .fixed{width:100%;height:50px;background:#000;opacity:0.8;position:fixed;}
        </style>
        <!--[if IE 6]>
        <script>
        //IE6下兼容fixed
        window.onscroll=function(){
          var doc=document.documentElement;
          var ypos=doc.scrollTop;
          var fix=document.getElementById("fixed");
          fix.style.position="absolute";
          fix.style.top=ypos;
        }
        </script>
        <![endif]--> 
        </head>
        <body>
        <div class="fixed" id="fixed"></div>

  透明度兼容问题

    描述：FF和chrome等支持opacity:0.5,而IE不兼容
    
    解决方法：`div{background:#000;opacity:0.5;filter:alpha(opacity=50);}`

    > 这个方法会让子元素同时透明，
    
    不让子元素透明解决方法
    
        div{background:#000;background:rgba(0,0,0,0.5);filter:alpha(opacity=50);}
        div *{position:relative;}  //设置直接子元素更好
        
    如果div元素本身设置了position:absolute或fixed则子元素恢复透明
    
      解决方法：在div外层新建一个包含层，用来设置定位absolute或fixed;
      
      另一种方法是新建一个透明层
      
          .box{width:100%;height:30px;position:relative;}
          .filter{background:#000;opacity:0.5;filter:alpha(opacity=50);postion:absolute;top:0;left:0;}
          <div class="box">
            <div class="filter"></div>
            <div class="con"></div>
          </div>
        
      最佳方法(通过IE滤镜实现)：
      
          div{
            background:rgba(0,0,0,0.5);
            filter:progid:DXImageTransform.Microsoft.gradient(startcolorstr=#7F000000,endcolorstr=#7F000000);
          }
          
         *透明滤镜在IE中存在问题，可以选中透明层下方元素，会触发mouseout事件
         
               filter:progid:DXImageTransform.Microsoft.gradient(startcolorstr=#7F000000,endcolorstr=#7F000000);

  IE6 png图片透明失效
  
    解决方法：引入pngFixed.js (@前端插件)
    
        <!--[if IE 6]>
        <script type="text/javascript" src="js/pngFixed.js"></script>
        <![endif]-->
        
      再给需要透明的元素加上类名pngFix
      
      支持图片,背景图片,css-sprite,平铺,hover伪类等

  IE 鼠标经过元素后高度发生变化
  
    触发条件：？

    解决方法：可为IE单独设置高度，并设置overflow:hidden

  overflow在IE6,7,8中失效BUG
  
    描述：在IE中，当父元素的直接子元素或者下级子元素的样式拥有position:relative属性时，父元素的overflow:hidden属性就会失效。
    
    解决方案：设置父元素position:relative可解决.（为设置overflow属性的元素添加position:relative）

  最小宽度和最小高度min-width和min-height在IE6中不兼容

    min-width和min-height的表现：
  
    1.起始值为设置的最小宽度或高度

    2.可以根据内容的增加自动伸长

    最小宽度解决方法：貌似只可以通过js解决
    
        var obj=document.getElementById("box");
        obj.style.width=obj.clientWidth>300?"auto":"300px";
        
    最小高度解决方法：`_height:需要设置的最小高度;	//IE6高度会自动撑开，不可行时设置overflow:visible;height:auto;`

        div{width:300px;min-height:300px;word-wrap:break-word;border:1px solid #ccc;_height:100px;}

  清除IE中空div默认高度
  
    解决方法：设置div{*line-height:0;}

  清除IE6块级元素默认高度
  
    div{_font-size:0;overflow:hidden;}

5. 清除浮动：float

方法一：设置`div{overflow:hidden;zoom:1;}`

方法二：最佳实践

        .clearfix:after{content:".";display:block;height:0;visibility:hidden;clear:both;}
        .clearfix{*zoom:1;} //IE6下不支持:after伪元素，需要触发hasLayout属性
        参考：那些年我们一起清除过的浮动

	`<hr>`块级元素


   自定义设置：
          
           hr{width:90%;margin:0 auto;height:1px;border:none;background:#ccc;color:#ccc;} /*设置color为了兼容IE*/

> 可能会有默认margin，慎用

处理方法：
        
        hr{width:90%;margin:0 auto;height:1px;background:#ccc;color:#ccc;*margin-bottom:-14px;*float:left;}

`<dl><dt><dd>`块级元素，适用于图文双排、问题答案等双排（标签语义化），可以通过设置dt,dd{float:left;}和dt,dd{display:inline-block;}实现水平排列。

display:inline和display:inline-block的区别：都是让块级元素像行内元素一样排列，但是inline是完全变成行内元素（不能设置宽高），而inline-block变成

行内元素的同时可以设置宽高,

（IE678对display:inline-block支持不好，想要实现和它一样的效果可以用以下代码）

  方法一：`div{display:inline-block;width:200px;} div{_display:inline;}`通过设置inline-block可以触发layout。

  方法二（常用）：`div{display:inline-block;*display:inline;*zoom:1;width:400px;}`直接设置为inline，然后通过zoom触发layout。

  兼容问题:

    display:inline-block设置元素为行内元素后会留下间隙，这是由于标记格式问题产生空格

    解决方法:
  
      1.设置父元素的font-size:0;在自身重新添加font-size样式（实用）
      
      2.去除html标记间的空格

行内元素和浮动的区别：存在兼容性问题，在IE6中如果行内元素放在浮动元素前面，会导致换行（浮动元素不会霸占行内元素的位置），

  总结：无论是行内浮动元素还是块级浮动元素和行内元素放在一起时都要让行内元素放在浮动元素后面。

`<table>`块级元素，适用于数据等表格式排列，默认无边框，可以通过border:1px solid #000;设置边框。可以通过设置border-collapse:collapse;设置边框合
成。设置横跨几列或者纵跨几行可以设置td的colspan和rowspan属性。

`<form>`块级元素，用于存放表单。

`<input><textarea>`行内元素但可以设置宽高。

> 一般行内元素都不可以设置宽高，但可以通过让行内元素设置浮动或者设置display:block(变成块级元素),display:inline-block实现。

`<textarea>`可以通过vertical-align:middle;让文字排在文本域垂直中间位置，类似的还有`<img><select>`标签

`<fieldset>`块级元素，默认黑色边框，可以通过css样式修改，常配合`<legend>`使用。

如何让图片垂直居中(不设置padding或margin值，不通过定位)

        <div class="box"><img src="" /></div>

解决方法：

        //非IE浏览器
        
        .box{width:200px;height:200px;line-height:200px;*font-size:0.873*容器高度;}
        
        .box img{vertical-align:middle;}
        
        //IE浏览器hack 兼容IE6
        
        .box{display:table-cell;vertical-align:middle;_font-size:0.873*容器高度;}
        
        .box img{vertical-align:middle;}

z-index属性在IE中兼容性问题

  问题描述：在IE中弹出层（比如二级菜单）z-index无论设置多大都会被其他设置position:relative的元素盖住

  解决方法：设置弹出层的最顶级父元素（除body,html）的z-index属性9999即可。

scrollTop兼容性问题

  问题描述：在IE和FF中，document.body.scrollTop总为0，在chrome中`document.documentElement.scrollTop`总为0；

  解决方法：`var scrollTop=(document.body.scrollTop==0)?document.documentElement.scrollTop:document.body.scrollTop;`

查询网页的模式

  `document.compatMode`
  返回CSS1Compat(标准模式)或BackCompat(混杂模式)

document.body和document.documentElement区别详解

  document.body是DOM对象body元素子节点，即`<body>`
  
  document.documentElement是整个DOM树根节点，即`<html>`

  区别主要在获取浏览器尺寸和滚动高度,兼容所有浏览器

  浏览器尺寸：

  怪异模式下(BackCompat)

          var width=document.body.clientWidth;
          var height=document.body.clientHeight;

  标准模式下(CSS1Compat)
  
        var width=document.documentElement.clientWidth;
        var height=document.documentElement.clientHeight;

  滚动高度：

  怪异模式下
  
        var scrollTop=document.body.scrollTop  //ff和chrome均有值，IE6下为0
        var scrollTop=document.documentElement.scrollTop //均为0
        
  标准模式下
  
        var scrollTop=document.body.scrollTop  //ff和IE6均为0，chrome有值
        var scrollTop=document.documentElement.scrollTop  //ff和IE6有值，chrome为0

  总结：一般都会声明标准模式，所以需要获取浏览器尺寸的就使用document.documentElement.clientHeight

  获取滚动高度兼容 `var scrollTop=(document.body.scrollTop==0)?document.documentElement.scrollTop:document.body.scrollTop;`

margin上下合并问题

  描述：当两个分别拥有margin-bottom和margin-top的块级元素垂直排列时会发生合并，结果为margin值较大的那个。

  注意：浮动元素不会发生合并（脱离文本层的元素不会和任何元素发生合并）

  设置clear属性的元素不会发生合并

jQuery在chrome中获取图片高度`$("img").height()`值一直为0

  问题：在chrome中，`$(document).ready()`加载图片时获取不到高度,只会加载dom树

  解决：
           
           $(window).load(function(){
              $("img").height();
            })

让一个绝对定位的元素自动撑开父元素

        <div class="box">
          <div class="con"></div>
        </div>
        .box{width:800px;position:relative;}
        .con{width:300px;position:absolute;}
        <script>
        $(function(){
          var height=parseInt($(".con").height())+parseInt($(".con").css("top"));
          $(".box").height(height+"px");
        })
        </script>
        *兼容IE6+

使用原生js获取嵌入样式和外部样式的属性

        function getStyle(obj,attr){
          if(obj.currentStyle){
            //IE
            return obj.currentStyle[attr];
          }else{
            //FF,chrome等
            return document.defaultView.getComputedStyle(obj,null)[attr];
          }
        }
        //document.defaultView.getComputedStyle非IE浏览器有的方法，可以接受两个参数
        
obj 获取样式的目标元素

  伪元素字符串 没有就写null

  > 兼容IE6+,如果未声明则获取不到
  
  obj.style.cssText可以读取所有css样式（仅限行内样式）

获取浮动样式

        非IE：obj.style.cssFloat;
        IE：obj.style.styleFloat;
        
兼容性写法：

        var _float=obj.style.cssFloat||obj.style.styleFloat;
  如：
  
        var obj=document.getElementById("box");
        var arr=[];
        var str1=obj.style.width;
        var str2=obj.style.height;
        var str3=obj.style.cssFloat||obj.style.styleFloat;
        var str4=obj.style.backgroundColor;
        arr.push(str1,str2,str3,str4);
        alert(arr);

文件按钮美化 `<input type="file" />`,兼容IE6+

  原理：通过定位把文件按钮放在button上，然后设置file按钮透明度为0
  
        <div class="box">
          <input type="text" class="text" id="text"/>
          <input type="file" class="file" onchange="document.getElementById('text').value=this.value" />
          <input type="button" class="btn" value="选择" />
        </div>
        .box{display:inline-block;*display:inline;*zoom:1;height:30px;margin:0 auto;position:relative;}
        .text{border:1px solid #ccc;width:200px;height:30px;color:#000;text-overflow:ellipsis;white-space:nowrap;overflow:hidden;}
        .btn{border:1px solid #ccc;height:30px;width:50px;color:#000;background:none;}
        .file{width:50px;font-size:10px;height:30px;position:absolute;right:0;top:0;opacity:0;filter:alpha(opacity=0);}
        
  > 在IE中无法通过width改变file的大小，只能通过设置font-size

`<img src="" /><input type="text" /><input type="submit" />`清除默认边框兼容性写法{border:0 none;}

自定义checkbox选项框

  IE6下select控件美化(*问题：箭头部分不能点击T_T)
  
        //js
        function reSelect(){
          var _width;
          $("select").each(function(){
            $(this).wrap("<span class='s-wrap1'></span>");
          });
          $(".s-wrap1").each(function(){
            $(this).wrap("<span class='s-wrap2'></span>");
            _width=parseInt($(this).find("select").width());
            $(this).width(_width-17+"px");
          });
          $(".s-wrap2").each(function(){
            _width=parseInt($(this).find("select").width());
            $(this).width(_width+"px");
          });
        }
        reSelect();
        //css
        /*select*/
        .s-wrap1,.s-wrap2{display:inline-block;vertical-align:middle;}
        .s-wrap1{overflow:hidden;background:none;}
        .s-wrap2{background:#fff url(../images/xjt.png) no-repeat right center;border:1px solid #ccc;}

onchange、onpropertychange、oninput事件区别

触发条件:
  
  onchange在对象属性发生改变，通过鼠标或键盘不是由于脚本触发，并且对象失去焦点onblur时触发

  onpropertychange是IE的专有事件，只要对象属性发生改变就会触发该事件
  
  oninput是非IE的事件，只有对象value值发生改变才会触发该事件
  
  onpropertychange BUG

使用原生js获取数组元素键值

        var lists=document.getElementsByTagName("li");
        for(var i=0;i<lists.length;i++){
          lists[i].onmouseover=function(){
            alert("我的键值为"+i);
          }
        }
        //i的值一直为lists.length-1，错误，值未被保存，当函数执行时循环早已结束
  
  正确写法：
  
        var lists=document.getElementsByTagName("li");
        for(var i=0;i<lists.length;i++){
          lists[i].index=i;  //保存键值在对应元素的index属性里  为每个对象添加对应的index属性
          lists[i].onmouseover=function(){
            alert("我的键值为"+this.index);
          }
        }

多行文字垂直居中
文字在未知高度容器垂直居中
line-height等于0的思考

让元素高度填满屏幕方法：

        html,body{height:100%;}
        div{height:100%;}

原生js通过类选择器获取对象

  `getElementsByClassName()` 兼容IE9+
  
  `querySelector(className)` 兼容IE9+,用来匹配对应类的第一个元素，参数也可以是其他选择器

  `querySelectorAll(className)` 兼容IE9+,用来匹配对应类的所有元素，参数也可以是其他选择器

原生js封装类

      function getClass(classname){
        if(document.getElementsByClassName){
          return document.getElementsByClassName(classname);
        }else{
          var all=document.getElementsByTagName("*");
          var arr=new Array();
          for(var i=0;i<all.length;i++){
            if(all[i].className==classname){
              arr[arr.length]=all[i];
            }		
          }
          return arr;
        }
      }	
      
  存在一个问题:当同时有两个类名时在IE中出错

  解决方法：待解决

瀑布流

延时加载.js

事件专题

捕获--目标--冒泡
  
  > 捕获阶段在IE8以下不支持

  封装事件处理程序(包含绑定，移除，对象，目标，取消默认)，兼容IE6+
  
        var eventUtil={
          //绑定事件
          addHandler:function(obj,type,handler){
            if(obj.addEventListener){
              return obj.addEventListener(type,handler,false);
            }else{
              return obj.attachEvent("on"+type,handler);
            }
          },
          //移除绑定
          removeHandler:function(obj,type,handler){
            if(obj.removeEventListener){
              obj.removeEventListener(type,handler,false);
            }else{
              obj.detachEvent("on"+type,handler);
            }
          },
          //事件对象
          getEvent:function(event){
            return event?event:window.event;
          },
          //事件目标
          getTarget:function(event){
            return event.target||event.srcElement;
          },
          //取消默认
          preventDefault:function(event){
            if(event.preventDefault){
              event.preventDefault();
            }else{
              event.returnValue=false;
            }
          },
          //阻止冒泡
          stopPropagation:function(event){
            if(event.stopPropagation){
              event.stopPropagation();
            }else{
              event.cancelBubble=true;
            }
          },
          //获取滚轮方向，暂时忽略Opera 9.5版本以下浏览器
          getWheelDelta:function(event){
            return event.wheelDelta?event.wheelDelta:-40*event.detail;
          }
        };
        
> 注意作用域问题，在非IE中，this指向触发事件的对象，在IE中this指向window对象

  事件对象event，注意只有在事件触发后才会有event对象，之前要先调用eventUtil.getEvent方法。

  滚轮事件问题要想兼容FF和IE,Chrome等浏览器，需要同时绑定mousewheel和DOMMouseScroll两个事件

> 相关目标relatedTarget(DOM)和fromElement/toElement(IE)

  只适用于mouseover(fromElement)和mouseout(toElement)事件

  鼠标从一个元素进入另一个，主目标元素为进入的元素，相关目标则为离开的元素

  鼠标从一个元素离开，主目标元素为离开的元素，相关目标则为离开后进入的元素

  主目标为this==event.currentTarget事件注册的元素(不变) target事件发生的具体元素(可变)

如果上方元素浮动，下方元素不浮动，下方设置margin-top无效，在IE中有效

  原因：上方元素未闭合，没有轮廓

  解决方法：浮动元素用div包着，清除浮动clearfix

同一个容器里既有未浮动元素又有定位元素和浮动，margin-top要慎用，最好用margin-bottom或padding-bottom代替，或者将它们从一个容器分开

a.more优先级高于.more

trigger自动触发事件（模拟事件），包括浏览器自带事件和自定义事件

  triggerHandler和trigger类似，不过多了一个return false功能（阻止默认）

  说明：
    
    jQuery对象专有

        $(".myBtn").trigger("click");  //触发点击事件
        参数说明type(事件类型),[data](需要传递的数据，数组格式)
        //绑定事件并初始化
        $("p").bind("myEvent", function (event, message1, message2) {
          alert(message1 + ' ' + message2);
        });
        //触发事件
        $("p").trigger("myEvent", ["Hello","World!"]);
        结果：Hello World!

jQuery获取元素位置

   $(this).css("left")如果未声明left属性，在chrome和IE可能获取不到

  可以使用$(this).position().left获取

重写原型的问题

        function Person(){   //构造函数
        };
        Person.prototype={     //重写原型
          name:"Higher",
          age:22,
          sayName:function(){
            alert(this.name);
          }
        }
        var higher=new Person();  //创建实例
        higher.sayName()  //Higher,没有问题
        
但是当创建实例放在重写原型之前会出现错误，没有sayName方法

重写后会切断实例与原型的联系，相当于原型对象的重指向

新实例会继承构造函数和原型的属性和方法，优先继承构造函数

focus()和select()

支持表单元素,select()兼容IE6+,ff,chrome

但是focus()在IE中不起作用，主要因为元素没有加载完成，需要把它放在window.onload=function(){...}里

兼容IE6+,FF,chrome里focus()和select()表现一样

禁用表单元素设置disabled属性为true,此时数据不可传送,需要传数据可以设为只读

兼容IE6+

将表单元素设为只读 `<input type="text" readonly="true" />`

计时器的问题

setTimeout()等可以采用如下结构避免字符串的拼接

        setTimeout(function(){
          //js代码
        },interval);

移动元素字符串拼接问题可用闭包解决

        elem.movement=setTimeout(function(){
          animate(final_x,final_y,elem,interval)
        },interval);
        *完整代码
        function animate(final_x,final_y,elem,interval){
          if(!elem){
            return false;
          }
          if(!elem.style.left){
            elem.style.left='0';
          }
          if(!elem.style.top){
            elem.style.top='0';
          }
          var xpos=parseInt(elem.style.left);
          var ypos=parseInt(elem.style.top);
          if(elem.movement){
            clearTimeout(elem.movement);
          }
          if(xpos==final_x&&ypos==final_y){
            return false;
          }
          if(xpos>final_x){
            var dist=Math.ceil((xpos-final_x)/10);
            xpos-=dist;
          }
          if(xpos<final_x){
            var dist=Math.ceil((final_x-xpos)/10);
            xpos+=dist;
          }
          if(ypos>final_y){
            var dist=Math.ceil((ypos-final_y)/10);
            ypos-=dist;
          }
          if(ypos<final_y){
            var dist=Math.ceil((final_y-ypos)/10);
            ypos+=dist;
          }
          elem.style.left=xpos+"px";
          elem.style.top=ypos+"px";
          elem.movement=setTimeout(function(){animate(final_x,final_y,elem,interval)},interval);
        }
        //水平方向滑动
        function xslide(elem,speed,final_x){
            elem.style.position="relative";
            if(!elem.style.left){
              elem.style.left="0";
            }
            var xpos=parseInt(elem.style.left);
            var dist=Math.ceil((parseInt(final_x)-xpos)/10);
            xpos=xpos+dist;
            if(xpos==final_x){
              return true;
            }
            elem.style.left=xpos+"px";
            setTimeout(function(){
              xslide(elem,speed,final_x)		
            },speed);
          }

自定义属性需要在属性名前加data-,如data-info=""

未加用HTML-CORE获取,ff为undefined，但IE6可以获取到

加上后ff获取方法dataset.info，在IE中会报错（HTML5）

兼容性需要通过DOM方法获取

      getAttribute("data-info")

通过DOM方法获取class问题

  ff,chrome等可以获取到，但是IE获取不到，可以通过判断div.className是否存在获取

DOM方法判断元素某属性是否存在

  `hasAttribute(attr)`,但是不兼容IE6
  
  可以使用`getAttribute(attr)`代替
  
属性选择器，兼容IE6+，只能获取拥有属性的元素或元素属性等于特定值的元素

        function attrSelector(attr,val){
          var all=document.getElementsByTagName("*"),
            arr=[],
            arr2=[];
          for(var i=0;i<all.length;i++){
            if(attr!='class'){
              if(all[i].getAttribute(attr)){
                arr.push(all[i]);
                if(all[i].getAttribute(attr)==val){
                  arr2.push(all[i]);
                }
              }
            }else{
              if(all[i].className){
                arr.push(all[i]);
                if(all[i].className==val){
                  arr2.push(all[i]);
                }
              }
            }	
          }
          if(!val){
            return arr;
          }else{
            return arr2;
          }	
        }

nextSibling兼容性

        <div class="c01">
          <p></p>
        </div>
        <div class="c02">
          <p></p>
        </div>
在IE中class="c01"的div的nextSibling为class="c02"的元素

在ff和chrome等浏览器中是空格节点nodeType=3

兼容性案例：(getElement为自定义获取元素函数)

        function brother(s1,s2){
          var _s1=getElement(s1),
            _s2=getElement(s2),
            elem=null;
          for(var i=0;i<_s1.length;i++){
            for(var j=0;j<_s2.length;j++){
              if(_s1[i].nextSibling.nodeType==1){
                elem=_s1[i].nextSibling;  //IE
              }else{
                elem=_s1[i].nextSibling.nextSibling;  //FF,chrome
              }
              if(elem.nodeType==1&&elem==_s2[j]){
                return _s2[j];
              }
            }		
          }
        }

js包含四种事件

一般化的UI事件(UIEvents)：鼠标和键盘都继承自UI事件,DOM3级为UIEvent

一般的鼠标事件(MouseEvents)：DOM3级为MouseEvent,初始化方法initMouseEvent

一般的DOM变动事件(MutationEvents)：DOM3级为MutationEvent,初始化方法initMutationEvent

一般的HTML事件(HTMLEvents)：没有对应的DOM3级事件,初始化方法initEvent

> 键盘事件在DOM3级中才有

模拟鼠标事件(DOM)

        //选取对象
        var o=document.getElementById("elem");
        //绑定事件
        eventUtil.addHandler(o,"mouseover",function(){
          alert(this.tagName);
        });
        //创建事件对象
        var event=document.createEvent("MouseEvents");
        事件对象初始化
        event.initMouseEvent("mouseover",true,true,document.defaultView,0,0,0,0,0,false,false,false,false,0,null);
        //触发事件
        o.dispatchEvent(event);

        模拟HTML事件(DOM)
        var o=attrSelector("type","text")[0];
        eventUtil.addHandler(o,"focus",function(){
          this.value="success!";
        });
        var event=document.createEvent("HTMLEvents");
        event.initEvent("focus",true,false);
        o.dispatchEvent(event);

自定义事件

        //获取对象
        var o=attrSelector("type","text")[0];
        //绑定事件
        eventUtil.addHandler(o,"myEvent",function(e){
          var e=eventUtil.getEvent(e);
          this.value="aha!";
          alert("o:"+e.detail);  //e.detail里存放事件初始化时定义的数据
        });
        //创建自定义事件对象
        var event=document.createEvent("CustomEvent");
        //初始化事件对象,参数(事件类型,是否应该冒泡,事件是否可以取消,detail对象任意值存放在event对象的detail属性中)
        event.initCustomEvent("myEvent",true,false,"hello world!");
        //触发事件
        o.dispatchEvent(event);
        
判断是否支持

 `document.implementation.hasFeature("CustomEvents","3.0");`

 或`document.createEvent  //能力检测，推荐`

IE中的模拟事件

        //创建事件对象 
        var event=document.createEventObject();  //不需要参数
        //配置事件属性，没有相应的初始化方法
        event.bubbles=true;
        event.cancelable=false;
        event.detail="hello world!";
        //触发事件
        o.fireEvent("onclick",event);

input的type属性在IE中不允许通过js修改

********找清css公有样式（组件类）

********js重用性（构架）

QQ聊天代码

`<a href="tencent://message/?uin=781957583&Site=&Menu=yes"><img src="http://wpa.qq.com/pa?p=1:402073566:10" alt="点击这里给我发消息"></a>`

鼠标样式修改

鼠标图片格式.cur

  `div{cursor:url(xx.cur),auto}`

禁止复制屏蔽右键ctrl+C

        <style type="text/css">
        //针对火狐
        body{  
           -moz-user-focus:ignore;  
           -moz-user-select:none;  
        }
        </style>
        document.oncontextmenu=function(e){
          e=e||window.event;
          if(e.preventDefault){
            e.preventDefault();
          }else{
            e.returnValue=false;
          }
        }
        document.onkeydown=function(e){
          e=e||window.event;
          if(e.ctrlKey&&e.which==67)||(e.ctrlKey&&e.which==86)) {
            if(e.preventDefault){
              e.preventDefault();
            }else{
              e.returnValue=false;
            }
                }        
        }

event对象的位置clientX,pageX,offsetX,screenX,x比较

clientX相对于可视区域显示的坐标

pageX坐标从页面顶端和左端计算

在页面没有滚动的时候pageX和clientX相等

offsetX发生事件的地点在事件源元素的坐标系统中的x坐标

screenX相对于屏幕显示的坐标

x设置或获取鼠标指针位置相对于父文档的 x 像素坐标。 

> offsetX和x是IE专有

offsetX支持IE和chrome不支持火狐

火狐需要layerX代替(需把父元素设置position:relative)

缓存相关

> 指定网页在缓存中的过期时间，一旦网页过期，必须到服务器上重新调阅。

`<meta http-equiv="expires" content="Wed, 26 Feb 1997 08:21:57 GMT" />`
  
  禁止浏览器从本地机的缓存中调阅页面内容

`<meta http-equiv="pragma" content="no-cach" />`

数据类型检测

基本类型：typeof操作符

包含undefined类型、null类型、boolean类型、number类型、string类型和object类型

undefined:对未声明或未初始化的变量,派生自null

null:空指针对象,typeof null会返回object,或值为空返回null

object:通过var obj=new Object()可以创建一个实例

每个实例对象都包含下列属性和方法

Constructor指向构造函数的指针

hasOwnProperty(propertyName)检查属性是否在实例中而不是在原型中，只有在实例中才返回true

	构造函数内部也会返回true  继承先后：本身>构造函数>原型
  
isPrototypeOf(obj)检查传入的对象是否是该对象的原型

propertyIsEnumerable(propertyName)检查属性是否可用for-in来枚举

toString() 返回对象的字符串表示

valueOf() 返回对象的字符串,数值,布尔值表示

引用类型：instanceof

如arr instanceof Array

数组检测方法Array.isArray()支持IE9+,FF等

in操作符可以检测属性对象是否可以访问到属性，无论是在原型还是实例中

for in可以循环出对象所有可遍历属性

动态添加脚本文件

外部脚本

      var script=document.createElement("script");
      script.src=***.js;
      document.body.appendChild(script);
      内部脚本
      var script=document.createElement("script");
      var text=document.createTextNode(//js代码);
      script.appendChild(text);
      document.body.appendChild(script);
      *以上方法不支持IE
      IE
      var script=document.createElement("script");
      script.text="//js代码";
      document.body.appendChild(script);
      *兼容性处理
      function loadScript(code){
        var script=document.createElement("script");
        try{
          var text=document.createTextNode(code);	
          script.appendChild(text);
        }catch(ex){
          script.text=code;
        }
        document.body.appendChild(script);
      }

动态添加样式文件

外部样式和script类似不过需要添加到head里

内部样式

        function loadStyle(css){
          var style=document.createElement("style");
          try{
            var text=document.createTextNode(css);
            style.appendChild(text);
          }catch(ex){
            style.styleSheet.cssText=css;
          }
          var head=document.getElementsByTagName("head")[0];
          head.appendChild(style);
        }

动态添加表格

        HTML CODE
        var table=document.createElement("table");
        var tbody=document.createElement("tbody");
        table.appendChild(tbody);
        //创建第一行
        tbody.insertRow(0);
        tbody.rows[0].insertCell(0);
        tbody.rows[0].cells[0].appendChild(document.createTextNode("内容"));
        tbody.rows[0].insertCell(1);
        tbody.rows[0].cells[1].appendChild(document.createTextNode("内容"));
        //创建第二行
        tbody.insertRow(1);
        tbody.rows[1].insertCell(0);
        tbody.rows[1].cells[0].appendChild(document.createTextNode("内容"));
        tbody.rows[1].insertCell(1);
        tbody.rows[1].cells[1].appendChild(document.createTextNode("内容"));
        //添加表格
        document.body.appendChild(table);

function类型

每个function类型的实例都包含两个对象：arguments和this

arguments是保存函数所有参数的类似数组的对象，有个callee的属性是个指针指向拥有这个arguments对象的函数，常用作递归

this引用的是函数的执行环境

如 say() -> window

person.say() -> person

say.apply(person,arguments) -> person

另外ECMAScript5规定了函数对象的caller属性指向调用这个函数的函数对象，在全局调用则为null

BOM

window对象

带有框架的页面frameset:每个框架都包含自己的window对象，保存在frames集合里，每个window对象都包含name属性，调用第一个框架window.frames[0]或top.frames[0]

确定窗口位置：var xpos=(typeof window.screenLeft=="number")?window.screenLeft:window.screenX; //兼容FF
var ypos=(typeof window.screenTop=="number")?window.screenTop:window.screenY; //兼容FF

移动窗口位置：window.moveTo(20,20);  //移动到坐标(20,20)处

window.moveBy(20,20);  //向下移动20px,向右移动20px

移动窗口可能会被浏览器禁用

获取可视窗口大小兼容：

        if(document.compatMode=="CSS1Compat"){  //标准模式
          var clientx=document.documentElement.clientWidth;
          var clienty=document.documentElement.clientHeight;
        }else{
          var clientx=document.body.clientWidth;
          var clienty=document.body.clientHeight;
        }
        
location对象

保存当前文档的信息

location.hash 返回URL中的hash(#后面的内容)，如果没有返回空字符串

location.host 返回服务器名称和端口号

location.hostname 返回不带端口号的服务器名称

location.href 返回完整的url

location.pathname 返回url中的目录或文件名

location.port 返回端口号

location.protocol 返回页面协议

location.search 返回url的查询字符串(以问号开头)

查询字符串参数

      function queryUrlString(){
        var URLString=window.location.search.length>0 ? location.search.substring(1) : "",
        URLArr=URLString.length ? URLString.split("&") : [],
        arg=[],
        items=null,
        sName=null,
        sValue=null;
        for(var i=0;i<URLArr.length;i++){
          items=URLArr[i].split("=");
          sName=decodeURIComponent(items[0]);
          sValue=decodeURIComponent(items[1]);
          if(name.length){
            arg[sName]=sValue;
          }
        }
        return arg;
      }
      
位置操作

      location.href="",window.location=""和location.assign("")效果一样，加载到指定页面，并在历史记录中生成一条记录
      location.replace("") 跳转到指针页面不会生成记录，后退键不可用
      location.reload() 重新载入页面，传递参数true强制从服务器加载页面
      
navigator对象

保存浏览器本身信息

通常用来检测插件和注册处理程序，也可以识别浏览器信息(不准确)

screen对象

保存浏览器窗口和显示器信息

history对象

保存用户上网记录

      history.go(-1) //后退一页
      history.go(1) //前进一页
      history.go("//url") 跳转到浏览记录里的url，如果不存在什么都不做
      history.back() //后退一页
      history.forward() //前进一页
      history.length  //返回历史记录的数量

原型和构造函数方法区别

  构造函数的方法每次new出来一个实例都会为他生成一个方法
  
  原型的方法是共享的，每次通过构造函数new出来的实例都会共用一个方法

IE中mouseover和mouseout事件会多次触发,不是冒泡

  常见问题二级菜单闪烁

  目标target和相关目标relatedTarget(DOM)、toElement/fromElement(IE)

  关键：判断相关目标

  判断一个元素是否包含另一个元素
  
        function contains(parentNode,childNode){
          if(parentNode.contains){
            return parentNode!=childNode && parentNode.contains(childNode);
          }else{
            return parentNode.compareDocumentPosition(childNode)==16;
          }
        }

IE6,7,8中获取select.value的值为空字符串

解决方法：为保证兼容最好为每个option都添加value属性

        <select>
          <option>1</option>
          <option>2</option>
          <option>3</option>
        </select>
ff,chrome,IE9+下 select.value可以获取

IE6,7,8 select.value=''

解决：

      <select>
        <option value='1'>1</option>
        <option value='2'>2</option>
        <option value='3'>3</option>
      </select>

获取选中option的文本内容

解决方法：

      var select=document.getElementsByTagName("select")[0];
      select.onchange=function(){
        alert(this.options[this.selectedIndex].innerHTML)
      }

jQuery队列方法queue&&dequeue&&clearQueue&&delay

queue方法用来添加事件队列 参数：queueName(自定义队列名称默认fx),newQueue(新队列),callback()(要添加进队列的函数)

dequeue方法用来执行队列中的下一个事件 参数：queueName(队列名称默认fx)

新建事件队列：

        var FUNC=[
          function() {$("#block1").animate({left:"+=100"},aniCB);},
          function() {$("#block2").animate({left:"+=100"},aniCB);},
          function() {$("#block1").animate({left:"+=100"},aniCB);},
          function() {$("#block2").animate({left:"+=100"},aniCB);},
          function() {$("#block1").animate({left:"+=100"},aniCB);},
          function(){alert("动画结束")}
        ];
        var aniCB=function() {
          $(document).dequeue("myAnimation");
        }
        $(document).queue("myAnimation",FUNC);
        aniCB();
        
清空队列两种方法：

      $(dom).queue('queuename',[]);
      $(dom).clearqueue('queuename');
      
delay方法：(延时执行)

      $("#block1").delay(4000,'myQueue').queue('myQueue',function(){});

横向无缝滚动

      $(function(){
        var i=0,
            $obj=$(".news-slide ul"),
            $elem=$(".news-slide"),
            $lists,
            _width=337;
            $elem.get(0).scrollLeft='0';
        function mover(){
          if(i==3){
            $lists=$elem.find("li").clone();
            $lists.appendTo($obj);
          }
          if(i==6){
            $elem.get(0).scrollLeft='0';
            $lists.remove();
            i=0;
          }
          i++;
          $elem.animate({"scrollLeft":"+="+_width+"px"},500);
        }
        $elem.get(0).mover=setInterval(function(){
          mover();
        },5000);
      })

添加收藏

IE：

    window.external.AddFavorite(url,title);

FF:

为a标签添加属性rel="sidebar"即可

    <a href="http://www.test.com" title="测试" rel="sidebar">添加收藏</a>

整合代码

设置  `<a href="url" title="title" rel="sidebar">添加收藏</a>  //ff`

      function addFavorite() {
          var url = window.location,
            title = document.title;
          try{
          window.external.addFavorite(url, title);  //IE
          }catch(e){
            if(!window.sidebar){
              alert('您的浏览器不支持,请按 Ctrl+D 手动收藏!');  //chrome,opera等
            }
          }
          return false;
      }

判断到达浏览器底部

      scrollTop+document.getElementsByTagName('body')[0].clientHeight>=document.getElementsByTagName('body')[0].scrollHeight;

document.all不可以用来检测是否是IE,虽然IE低级版本支持，但opera浏览器也支持

        //图片轮播(fade)
        (function(){
          var $main=$('.show-slide li'),  //目标li
            i=0,
            m=0,
            len=$main.length,
            $obj=$('.show-slide-box'), //存放容器
            $point,
            $btn;
          createPoint($obj);
          $btn=$('.pointer li');
          $main.eq(0).show().siblings().hide();
          $btn.eq(0).addClass('selected').siblings().removeClass('selected');
          function main($main){
            if(!$main.is(':animated')){
              if(i>=len-1){
                i=0;
              }else{
                i++;
              }
              $main.eq(i).fadeIn(500).siblings().fadeOut(500);
              $btn.eq(i).addClass('selected').siblings().removeClass('selected');
            }	
          }
          $main.get(0).mover=setInterval(function(){
            main($main);
          },8000);
          function createPoint($obj){
            $point=$("<ul class='pointer'></ul>");
            for(m=0;m<len;m++){
              $("<li></li>").appendTo($point);
            }
            $point.appendTo($obj);
          }
          $btn.bind('mouseover',function(){
            clearInterval($main.get(0).mover);
            i=$btn.index($(this));
            $main.eq(i).fadeIn(500).siblings().fadeOut(500);
            $btn.eq(i).addClass('selected').siblings().removeClass('selected');
            $main.get(0).mover=setInterval(function(){
              main($main);
            },8000);
          });
        })();

超大未知宽度图片如何在容器里水平居中

    <div>
      <span>
        <img src="" />
      </span>
    </div>
    css:
    div{position:relative;overflow:hidden;}
    span{position:absolute;display:block;text-align:center;width:3000px;top:0;left:50%;margin-left:-1500px;} //保证宽度足够大
    img{height:300px;width:auto;}

未知高度图片如何在未知高度容器垂直居中(IE8+)

      <div>
        <span>
          <img src="" />
        </span>
      </div>
      css:
      div{display:table;}
      span{display:table-cell;vertical-align:middle;}
      img{width:100%;height:auto;}

做到IE67兼容设置`div *font-size:0.873*容器高度`

可视化编辑基础

使页面区域可编辑两种方法：

1.iframe

目标页面test.html

编辑页面.html

    <iframe name="editable" style="width:200px;height:200px;" src="test.html"></iframe>
    <script>
    //设置编辑属性为on
    iframes['editable'].document.designMode='on';
    </script>
    
2.contenteditable属性

    <div contenteditable='true'>测试文字</div>
操作富文本命令

1.应对第一种方式

    iframes['editable'].document.execCommand('命令名称',false,'附加参数,没有为null');
2.应对第二种方式

    document.execCommand('命令名称',false,'附加参数,没有为null');
  一般应有触发事件
  
  例：iframes['editable'].document.execCommand('createlink',false,'http://www.baidu.com');
  
富文本内容发送至服务器

  方法：将内容添加到隐藏字段中通过submit来提交

ajax

优点：

1.减轻带宽压力

2.增加用户体验

缺点：

1.破坏浏览器前进后退按钮

2.不利于seo

原生js实现ajax

      <button id="btn">加载名言</button>
      <div id="show"></div>
      <script>
      window.onload=function(){
        var btn=document.getElementById('btn'),
          box=document.getElementById('show'),
          xhr='';
        if(window.XMLHttpRequest){
          xhr=new XMLHttpRequest();
        }else{
          xhr=new ActiveXObject("Microsoft.XMLHTTP");
        }
        btn.onclick=function(){
          xhr.open('GET','test.txt',true);
          xhr.onreadystatechange=xhrCallback;
          xhr.send(null);
        }
        function xhrCallback(){
          if(xhr.readyState==4 && xhr.status==200){
            box.innerHTML=xhr.responseText;
          }
        }
      }
      </script>
      
jquery中的ajax

1.load方法

语法：$(selector).load(url,[data],[callback]);

说明：

①可以对加载的html文档进行筛选，如$(selector).load('test.html .comment');  //只加载test.html文档里class为comment的内容

②根据有无data参数自动转换为get或post方式

③回调函数的参数responseText,textStatus,XMLHttpRequest

④回调函数在请求完成时触发，无论成功或者失败

例：

      $('#resText').load('test.html',{name:"yy",age:"22"},function(){
        //responseText ：请求返回的内容
        //textStatus ：请求状态：success/error/notmodified/timeout
        //XMLHttpRequest ：XMLHttpRequest对象
      })

2.$.get()方法和$.post()方法

语法：$.get(url,[data],[callback],[type]);

说明：

①data在get时会自动转换为?key1=val1&key2=val2(可见);post时会作为HTTP消息发送给服务器(不可见)

②回调函数只会在请求成功后调用,和load不同

③回调函数的参数data,textStatus

④data的值可以使用表单序列化方法，如$.get('get1.php',$('#form').serialize(),function(){
	//回调函数
})

⑤type为需要返回的数据类型，只有返回类型为json时需要加上，此外期待返回类型为xml时需要服务器设置content-type类型，header("Content-
Type:text/xml;charset=utf-8");

例：

      $('#btn').click(function(){
        $.get('get1.php',{
          username:$('#username').val(),
          content:$('#content').val()
        },function(data,textStatus){
          //data ：返回的内容,可以是XML文档/JSON文件/HTML片段
          //textStatus ：请求状态
        })
      })
      
3.$.getScript()方法

语法：$.getScript(url,[callback]);

说明：

①url为需要加载的js文件路径

②回调函数会在请求成功后调用

如：

    $('#btn').click(function(){
        $.getScript('test.js',function(){
          alert('success');
        });
    })
    
4.$.getJSON()方法

语法：$.getJSON(url,[callback]);

说明：

①url为需要加载的json文件路径

②回调函数会在请求成功时调用

③回调函数的参数data //返回的数据

④$.each()方法可以循环操作数组和对象,循环对象需要包含两个以上否则会返回undefined

例：

    $('#btn').click(function(){
      $.getJSON('test.json',function(data){
        $.each(data,function(i,comment){
          txt+="<div class='comment'><h2>"+comment['username']+"</h2><p>"+comment['content']+"</p></div>";
        })
        $('#resText').html(txt);
      })
    });
    
5.$.ajax()方法

语法：$.ajax(options);

说明：

①常用参数：

      url //发送请求的地址[string]
      type //请求方式[string]
      timeout //请求超时时间[number]
      data //发送到服务器的数据[object/string]
      dataType //预期服务器返回的数据类型[string],包含json,xml,html,script,jsonp,text
      beforeSend //请求前处理函数[function]
      complete //完成处理函数,不管成功与否[function]
      success //成功处理函数[function]
      error //请求失败处理函数[function]
      global //是否触发全局ajax事件，默认为true[boolean]

例：

    $('#btn').click(function(){
      $.ajax({
        type:"GET",
        url:"test.json",
        timeout:5000,
        dataType:"json",
        beforeSend:function(XMLHttpRequest){
          //请求前处理函数(可以取消请求或添加自定义HTTP头)
        },
        error:function(XMLHttpRequest,textStatus,[errorThrown]){
          //请求失败处理函数
        },
        complete:function(XMLHttpRequest,textStatus){
          //完成处理函数,不管成功与否
        },
        success:function(data){
          //成功处理函数
        }
      });
    })
    
附json和xml数据调用实例
①

      <!--test.json-->
      {
        "1":{
          "name":"yy",
          "age":"22",
          "family":["11","22"]
        },
        "2":{
          "name":"pp",
          "age":"21",
          "family":["dd","mm"]
        }
      }
      
      <!--getjson.html-->
      <!DOCTYPE HTML>
      <html>
      <head>
      <meta http-equiv="content-type" content="text/html;charset=utf-8" />
      <script src="js/jquery.js"></script>
      </head>
      <body>
      <div id="box"></div>
      <button id="btn">获取数据</button>
      <script>
      $(function(){
        var $btn=$('#btn'),
          $box=$('#box');
        $btn.click(function(){
          var txt='';
          $.ajax({
            type:"GET",
            url:"test.json",
            dataType:"json",
            success:function(data){
              $.each(data,function(i,comet){
                txt+="<h1>"+comet['name']+"</h1><h2>"+comet['age']+"</h2><h3>"+comet['family']+"</h3>";
              });
              $box.html(txt);
            }
          });
        });
      })
      </script>
      </body>
      </html>
      
②

      <!--test.xml-->
      <bookstore>
        <book category="CHILDREN">
          <title>Harry Potter</title> 
          <author>J K. Rowling</author> 
          <year>2005</year> 
          <price>29.99</price> 
        </book>
        <book category="WEB">
          <title>Learning XML</title> 
          <author>Erik T. Ray</author> 
          <year>2003</year> 
          <price>39.95</price> 
        </book>
      </bookstore>
      <!--getxml.html-->
      <!DOCTYPE HTML>
      <html>
      <head>
      <meta http-equiv="content-type" content="text/html;charset=utf-8" />
      <script src="js/jquery.js"></script>
      </head>
      <body>
      <div id="box"></div>
      <button id="btn">加载数据</button>
      <script>
      $(function(){
        var $btn=$('#btn'),
          $box=$('#box');
        $btn.click(function(){
          var txt='';
          $.ajax({
            type:"GET",
            url:"test.xml",
            dataType:"xml",
            success:function(data){
              var title='',
                cate='';
              $(data).find("book").each(function(){
                title=$(this).find("title").text();
                cate=$(this).attr("category");
                txt+="书名："+title+"分类："+cate+"<br />";
              })
              $box.html(txt);
            }
          });
        });
      })
      </script>
      </body>
      </html>
      
6.ajax的全局事件(只要存在ajax就会触发)

ajax请求开始时会触发ajaxStart()方法的回调函数

请求结束时会触发ajaxStop()方法的回调函数

另外几种方法：ajaxComplete(callback)请求完成时触发 ajaxError(callback)请求错误时触发 ajaxSend(callback)请求前触发 ajaxSuccess(callback)请求成功时触发

如：提示信息(加载中...)

      <div id="loading">加载中...</div>
      <script>
      $('#loading').ajaxStart(function(){
        $(this).show();
      }).ajaxStop(function(){
        $(this).hide();
      })
      </script>
      
7.跨域的jsonp方法

原理：

语法：

      //前端
      $.ajax({
        type:"GET",
        url:目标网址,
        dataType:"jsonp",
        jsonp:"callback",
        success:function(){
          //成功处理函数
        }
      });
      //后台
      $jsonp=$_GET["callback"];
      
输出$jsonp("json字符串")格式

方法只能是GET不能是POST

序列化元素

1.serialize()方法

语法：$(selector).serialize();

说明：

①返回字符串

②常用于表单元素，不过其他DOM元素也可以使用

例：

      $('#form').serialize(); //将ID为form的表单里所有表单元素的值序列化
      $(':checkbox,:radio').serialize(); //将复选框和单选框的值序列化
      
2.serializeArray()方法

语法：$(selector).serializeArray();

说明：

①返回json格式数据

②返回的对象可以使用$.each()方法迭代

例：

      var fields=$(':checkbox,:radio').serializeArray();
      console.log(fields);
      
3.$.param()方法

语法：$.param(obj);

说明:是serialize()方法的核心，用来对一个数组或对象按照key/value进行序列化

例：

      var obj={a:1,b:2,c:3};
      var k=$.param(obj);
      alert(k);  //a=1&b=2&c=3

json数据都使用双引号不要使用单引号

如：

      {
        "hanyy":{
          "username":"yy",
          "age":"22"
        },
        "zhuxp":{
          "username":"pp",
          "age":"20"
        }
      }

jQuery插件

优势：复用，便于维护

插件种类：封装对象方法的插件 封装全局函数的插件 选择器插件

说明：

①基本框架

      ;(function($){
        //插件
      })(jQuery);
      
②插件内部this指向jQuery对象而不是DOM对象

③可以通过this.each()来遍历所有元素

④闭包中的函数可以通过$.Func=func变成全局函数

⑤第一种插件写法$.fn.extend(obj),二三种写法$.extend(obj)

⑥要返回jQuery对象,保证可链式操作,除非插件需要返回是一些需要获取的量

插件机制

$.extend()方法的用途，用来组合对象，覆盖默认设置

例：

      ;(function($){
        function foo(options){
          options=$.extend({
            name:"yy",
            age:22
          },options);
          return options;
        }
        $.Foo=foo;
      })(jQuery);
      $(function(){
        var $pp=$.Foo({name:"bb",age:20});
        alert($pp);  //{name:"bb",age:20}
      });
      
编写插件

①jquery.color.js

      ;(function($){
        $.fn.extend({
          "color":function(value){
            return this.css("color",value);
          }
        })
      })(jQuery);
      
//应用

      $(function(){
        alert($('.box').color("red"));  //obj
      });
      
②jquery.alterBgColor.js

      ;(function($){
        $.fn.extend({
          "alteBgColor":function(options){
            options=$.extend({
              odd:"odd",
              even:"even",
              selected:"selected"  //默认参数
            },options);
            $('tbody>tr:odd',this).addClass(options.odd);  //相当于$(this).find("tbody>tr"),用来选择$(this)对象中的tr元素
            $('tbody>tr:even',this).addClass(options.even);
            $('tbody>tr',this).click(function(){
              var hasSelected=$(this).hasClass(options.selected);
              $(this)[hasSelected?"removeClass":"addClass"](options.selected);
              $(this).find(':checkbox').attr('checked',!hasSelected);
            });
            $('tbody>tr:has(:checked)',this).addClass(options.selected);  //如果默认为选中状态,为它加上CLASS selected
            return this;  //返回jQuery对象,保证链式操作
          }
        })
      })(jQuery);
      //应用
      $(function(){
        $('.table2').alteBgColor();
      });


*************移动端****************

气泡css写法

原理：存在宽高的元素设置边框时会形成4个等腰梯形，如果元素宽高为0且只有一边边框有颜色时(其他边框颜色同于背景色或为透明),会形成箭头▲
例；

      <!--html-->
      <div class="test">
        <span class="bot"></span>
        <span class="top"></span>
      </div>
      <!--css-->
      .test{width:200px;height:50px;border:2px solid #d9d9d9;position:relative;margin-top:30px;}
      .test span{width:0;height:0;border-width:10px;border-style:solid;border-color:transparent transparent #d9d9d9;position:absolute;top:-20px;overflow:hidden;left:80px;}
      .test span.top{top:-18px;border-color:transparent transparent #fff;z-index:2;}

移动端事件

触摸事件：(包含鼠标事件的一切属性)

touchstart(无论是否已有手指都会触发)

touchmove(手指在屏幕滑动时连续触发,当存在滚动条时可通过event.preventDefault()阻止滚动)

touchend(手指从屏幕移开触发)

有三个跟踪触摸的属性

touches(当前跟踪触摸操作的Touch对象数组)

targetTouches(特定于事件目标的Touch对象数组)

changedTouches(自上次触摸发生改变的Touch对象数组)

每个Touch对象都包含属性：clientX,clientY,indentifier(标识触摸的唯一ID),pageX,pageY,screenX,screenY,target(触摸的DOM节点目标)

如：

      var box=document.getElementById('box'),
        handle=function(event){
          switch(event.type){
            case 'touchstart':
              box.innerHTML="touchstart"+event.touches[0].clientX;
              break;
            case 'touchmove':
              event.preventDefault();
              box.innerHTML="touchmove"+event.changedTouches[0].clientX;
              break;
            case 'touchend':
              box.innerHTML="touchend"+event.changedTouches[0].clientX;
              break;
          }
        };
      box.addEventListener('touchstart',handle,false);
      box.addEventListener('touchmove',handle,false);
      box.addEventListener('touchend',handle,false);
      
手势事件：

    gesturestart(当一个手指已经按在屏幕另个手指又触摸屏幕时触发)
    gesturechange(当任何一只手指位置变化时触发)
    gestureend(当任何一只手指从屏幕移开时触发)
    
触发顺序：gesturestart(另一只手指放上触发)-touchstart-gesturechange-gestureend(任一只手指移开时触发)-touchend;

除了拥有标准鼠标事件的属性还包含额外属性：rotation(手指变化引起的旋转角度,顺时针为正值,逆时针为负值,从0开始)和scale(两手指间的距离,距离拉大增长,距离缩短减小,从1开始)

如：

      var box=document.getElementById('box'),
        handle=function(event){
          switch(event.type){
            case 'gesturestart':
              box.innerHTML='gesturestart(rotation:'+event.rotation+',scale:'+event.scale+')';
              break;
            case 'gesturechange':
              box.innerHTML='gesturechange(rotation:'+event.rotation+',scale:'+event.scale+')';
              break;
            case 'gestureend':
              box.innerHTML='gestureend(rotation:'+event.rotation+',scale:'+event.scale+')';
              break;
          }
        };
      box.addEventListener('gesturestart',handle,false);
      box.addEventListener('gesturechange',handle,false);
      box.addEventListener('gestureend',handle,false);
      
如何判断手指滑动方向和滑动大小

原理：

    touchstart:获取event.touches[0].pageX的值
    touchmove:获取event.changeTouches[0].pageX的值
    
然后比较两次的值来判断

code:

      var box=document.getElementById('box'),
        x_start,
        x_end;
      box.addEventListener('touchstart',function(event){
        x_start=parseInt(event.touches[0].pageX);
      },false);
      box.addEventListener('touchmove',function(event){
        x_end=parseInt(event.changedTouches[0].pageX);
        if(x_end>x_start){
          box.innerHTML='右滑';
        }else if(x_end<x_start){
          box.innerHTML='左滑';
        }else{
          box.innerHTML='未动';
        }
      },false);
      
jquery封装统一使用e.originalEvent.changedTouches[0].pageX获取

<!-------------------------LESS-------------------------------->

*****变量*****

包括：

①值变量

      @var:#333;
      .box{
        color:@var;
      }
      //result
      .box{
        color:#333;
      }
      
②选择器变量

    @var:box;
    .@{var}{
      color:#333;
    }
    //result
    .box{
      color:#333;
    }
    
③url变量

      @var:"../images";
      .box{
        background-image:url("@{var}/test.png");
      }
      //result
      .box{
        background-image:url(../images/test.png);
      }
      @themes:"../themes";
      @import "@{themes}/tidal-wave.less";
      
④属性变量

      @var:color;
      .box{
        @{var}:#333;
        background-@{var}:#fff;
      }
      //result
      .box{
        color:#333;
        background-color:#fff;
      }
插值

      @var:"hello";
      @hello:"hello world";
      .box{
        &:after{
          content:@@var;
        }
      }
      //result
      .box:after{
        content:"hello world";
      }
      
懒加载(作用域)

      .lazy-eval{
        width:@var;
        @a:10%;
      }
      @var:@a;
      @a:100%;
      //result
      .lazy-eval{
        width:10%;
      }

      @var:0;
      .class1{
        @var:1;
        .class{
          @var:2;
          three:@var;
          @var:3;
        }
        one:@var;
      }
      //result
      .class1 .class{
        three:3;
      }
      .class1{
        one:1;
      }
      
& (代表上级选择器)

    .box{
      &:after{
        content:"";
      }
    }
    //result
    .box:after{
      content:"";
    }
    .box{
      .text &{
        color:#333;
      }
    }
    //result
    .text .box{
      color:#333;
    }

*********继承********

生成的是组合是写法

如：

      .a{
        width:20px;
      }
      .b:extend(.a){
        color:#333;
      };
      //result
      .a,.b{
        width:20px;
      }
      .b{
        color:#333;
      }
属性继承

写法：(继承必须写在最后)

①.

      a{
        &:extend(.b);
      }
      
②.

      a:extend(.b){
        //只继承.b
      }
      
③.

    a:extend(.b all){
      //同时继承.x.b .b .b.x等
    }
    
④.

      a:extend(.b,.c){
        //相当于.a:extend(.b){} .a:extend(.c){}
      }
      
⑤

      pre:hover:extend(div pre){
        //中间可以跟空格 pre:hover :extend(div pre){}
      }
⑥

    pre:hover:extend(div pre):extend(.bucket tr){
      //相当于pre:hover:extend(div pre,.bucket tr){}
    }
    
⑦

      pre:hover,.some-class{
        &:extend(div pre);
      }
      //相当于pre:hover:extend(div pre),.some-class:extend(div pre){}
      *pre:hover:extend(div pre).nth-child(odd)(会报错)

当被继承选择器为变量时无法继承,但继承选择器为变量可以继承

如：


      @var:.box;
      @{var}{
        color:#333;
      }
      .text:extend(.box){
        //无法继承
      }
      
②

      .box{
        color:#333;
      }
      .text:extend(@{var}){
        //无法继承
      }
      @var:.box;
      
③

      .box{
        color:#333;
      }
      @variable:.text;
      @{variable}:extend(.text){
        //可以继承
      }

@media规则

①只能继承同一个media内部的元素属性

如：

      @media screen{
        .selector:extend(.box){

        };
        @media (max-width:400px){
          .box{
            width:200px;
          }
        }
      }
      //result
      @media screen and (max-width:400px){
        .box{
          width:400px;
        }
      }
      
②顶级的元素可以继承到所有包括在media里的元素属性

如：

      @media screen{
        .box{
          width:200px;
        }
        @media (max-width:400px){
          .box{
            width:20%;
          }
        }
      }
      .selector:extend(.box){
        //
      };
      //result
      @media screen{
        .box,.selector{
          width:200px;
        }
      }
      @media screen and (max-width:400px){
        .box,.selector{
          width:20%;
        }
      }

继承没有去重检测

一个元素同时继承多个元素的属性会导致重复

如：

      .box,.text{
        width:200px;
      }
      .btn-box:extend(.box,.text){};
      //result
      .box,
      .text,
      .btn-box,
      .btn-box {
        width: 200px;
      }

混合mixin

      <!--几个例子-->
      
错误的写法：

      .a(),.b(){
        width:20px;
      }
      .c{
        .a;
      }
      
正确的写法

      .a,.b{
        width:20px;
      }
      .c{
        .a();  //括号可以省略
        color:#333;
      }
      //result
      .a,.b{
        width:20px;
      }
      .c{
        width:20px;
        color:#333;
      }
      
不输出的混合

      .a(){
        width:10px;
      }
      .b{
        .a;
        color:#333;
      }
      //result
      .b{
        width:10px;
        color:#333;
      }

      .a{
        .b(){
          width:10px;
        }
        color:#333;
      }
      .c{
        .a>.b;
      }
      //result
      .a{
        color:#333;
      }
      .c{
        width:10px;
      }

包含选择器的混合

      .a(){
        &:hover{
          color:#333;
        }
      }
      .b{
        .a;
      }
      //result
      .b:hover{
        color:#333;
      }
      
混合中的命名空间Namespaces

      .a{
        .b{
          color:#333;
        }
      }
      .c{
        .a>.b;  //相当于.a>.b(); .a.b;  .a.b();
      }
      //result
      .a .b{
        color:#333;
      }
      .c{
        color:#333;
      }
      
命名空间可以防止与其它库冲突

      #name{
        .a(){
          color:#333;
        }
      }
      .c{
        #name>.a;
      }
      //result
      .c{
        color:#333;
      }
      
混合中的!important关键词

如：

      .a(){
        width:20px;
        color:#333;
      }
      .b{
        .a !important;
        width:10px;
      }
      //result
      .b{
        width:20px !important;
        color:#333 !important;
        width:10px;
      }

带有参数的混合mixin

例子：

      .radius(@var){
        border-radius:@var;
        -webkit-border-radius:@var;
        -moz-border-radius:@var;
      }
      .a{
        .radius(4px);
      }
      //result
      .a{
        border-radius:4px;
        -webkit-border-radius:4px;
        -moz-border-radius:4px;
      }
(下一种不常用)

      .radius(@var:5px){
        border-radius:@var;
        -webkit-border-radius:@var;
        -moz-border-radius:@var;
      }
      .a{
        .radius;
      }
      //result
      .a{
        border-radius:5px;
        -webkit-border-radius:5px;
        -moz-border-radius:5px;
      }



cookie VS localStorage

json

前端js解析json方法

用eval()方法解析json：eval("("+data+")");

$.parseJson()

后台php输出json方法

json_encode() 把数组生成json数据

json_decode() 解析json数据成数组

ajax VS comet && websocket

轮询 长连接 服务器发消息onmessage监听

200 404 302等代码意义


=>php


语音识别


css重用性

模块类 基类widget{}

扩展   widget-sidebar{}

复用

命名空间=>层级关系


要点：

公有+私有 命名空间

全局通用模块=>按钮 表单 小挂件(vip,new等) tip 弹窗 颜色 页码等

外层content{width:1000px;margin:auto;}

样式范围尽量缩小，尤其是通用样式，共用部分命名不要沿袭父层

控制样式的类和js类要分离

    /**
    命名：结构命名>作用
    问题：复用性 命名
    命名时不要附加页面内容名称 如：askList等,按结构命名 如：col-list
    容器(container) {width;padding;margin;position}
    头部(header)
    底部(footer)
    导航(nav)
    块分组(col)
    标题 title {font-size;}
    列表 ulist
    段落 p {font-size;line-height} 
    图像 pic {width;}
    表格 table
    表单 ipt
    菜单 menu

    单独 margin padding color fl fr center pa pf pr
    **/
    
可预测、可重用、可维护和可伸缩

CSS要定义的应该是组件的外观

布局和位置应当由一个单独的布局类或者单独的容器元素构成

基础包括了复位原则和元素缺省值；布局是对站点范围内的元素进行定位以及像网格系统那样作为一种通用布局助手；模块即是可重用的视觉元素；状态即指样式，可以通过JavaScript进行开启或关闭。

组件

2014/11/10 

1.class值：通用+特殊 最好不超过3个，样式差不多就可以引用，然后添加一个特殊样式即可

2.层级结构越少越好，防止样式被覆盖

最外层

      .box{
        width:100%;
        position:relative;
        background:;
      }
      外层容器
      .container{
        margin:auto;
        width:;
        padding:;
      }
      左浮动元素
      .fl{
        float:left;
      }
      右浮动元素
      .fr{
        float:right;
      }
      清除浮动
      .clearfix{*zoom:1;}
      .clearfix:after{
        content:".";
        height:0;
        display:block;
        clear:both;
        visibility:hidden;
      }
      定位框
      .widget{
        position:absolute;
        width:;
        padding:;
      }
      按钮
      .btn{
        width:;
        -ms-border-radius:4px;
        -webkit-border-radius:4px;
        -moz-border-radius:4px;
        border-radius:4px;
        background:;
        text-align:center;
        line-height:;
      }



不可用

fl,fr

可用

center

可重用

IE很多bug都是因为宽度不明，尤其浮动元素宽度，尝试为其设置宽度解决

或者边框不明确，尝试使用display:block;

浮动块级元素可能需要触发layout

奇葩IE8服务器图片不显示

解决方法：给外层元素设置尺寸




**************面向对象编程MVC****************

相关概念：类，实例，初始化，属性，方法

类的初始化

      var Class=function(){
        var _class=function(){
          this.init.apply(this,arguments);
        };
        _class.prototype.init=function(){};
        return _class;
      };
      
创建类

    var Person = new Class;
    
给类添加函数

    Person.addFunc=function(){};  //缺点，每新建一个实例都会重新创建一个方法
    
基于类的实例做初始化

    Person.prototype.init=function(){
      //基于Person的实例做初始化
    };
    
给类添加实例函数

    Person.prototype.addFunc=function(){};  //每个实例都共用一个方法
    
给类的prototype添加一个别名fn

    Person.fn=Person.prototype;
    
->添加方法：Person.fn.addFunc=function(){};

新建实例

    var person=new Person;
    
通过extend和include函数来添加类属性和实例属性

      var Class=function(){
        var _class=function(){
          this.init.apply(this,arguments);
        };
        _class.prototype.init=function(){};
        //定义 prototype 的别名
        _class.fn=_class.prototype;
        //定义类的别名
        _class.fn.parent=_class;
        //给类添加属性
        _class.extend=function(obj){
          var extended=obj.extended;  //添加回调函数
          for(var i in obj){
            _class[i]=obj[i];
          }
          if(extended) extended(_class)
        };
        //给实例添加属性
        _class.include=function(obj){
          var included=obj.included;
          for(var i in obj){
            _class.fn[i]=obj[i];
          }
          if(included) included(_class)
        };
        return _class;
      };
      
使用

      var Person=new Class;
      Person.extend({
        find:function(id) { /* ... */ },
        exists:functions(id) { /* ... */ }
      });
      var person=Person.find(1);
      
->for in可以遍历一个对象的所有属性和方法

  extend和include函数的参数为一个对象
  
  extended和included为回调函数
  
        Person.extend({
          //回调函数
          extended:function(_class) {
            console.log(_class," was extended!");
          }
        });
