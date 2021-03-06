# Event事件

​	事件是可以被JavaScript侦测到的行为。网页中的每个元素都可以产生某些可以触发JavaScript函数的事件.

​	事件是一瞬间触发。

### 事件绑定方式

​	DOM节点绑定（格式）：节点.on+事件名 = 事件处理函数;

```
//（1）DOM节点绑定
//	*同名事件会被覆盖
//  *事件处理函数只能冒泡阶段执行
div.onclick = function(){}
//（2）作为html属性
//<div onclick="sum()"></div> 不常用，不实用。
//（3）事件监听器
//	*同名事件不会被覆盖
//  *事件处理函数默认冒泡阶段执行
```

### 常用事件

#### 鼠标事件

onclick 当用户点击某个对象时调用的事件。
ondblclick 当用户双击某个对象时调用的事件。
onmousedown 鼠标按钮被按下。
onmouseup 鼠标按键被松开。
onmouseover 鼠标移到某元素之上。
onmouseout 鼠标从某元素移开。
onmousemove 鼠标被移动时触发。
onmouseenter 在鼠标光标从元素外部移动到元素范围之内时触发。这个事件不冒泡
onmouseleave 在位于元素上方的鼠标光标移动到元素范围之外时触发。这个事件不冒泡
oncontextmenu 鼠标右键菜单展开时触发。
PS：click = mousedown + mouseup, dblclick = click*2(短时间内两次单击);
执行顺序：mouseover=>mouseenter; mouseout => mouseleave

#### 键盘事件

onkeydown 某个键盘按键被按下。
onkeyup 某个键盘按键被松开。
onkeypress 键盘<字符键>被按下触发,而且如果按住不放的话，会重复触发此事件。

#### UI事件

onload 页面元素（包括图片多媒体等）加载完成后
onscroll 滚动时触发。
onresize 窗口或框架被重新调整大小。

#### 表单事件

onselect 输入框文本被选中。
onblur 元素失去焦点时触发。
onfocus 元素获得焦点时触发。
onchange 元素内容被改变，且失去焦点时触发。
onreset 重置按钮被点击。
onsubmit 确认按钮被点击。
oninput 输入字符时触发

##### 演示：常用事件

```
鼠标事件
* onclick
* onmousedown		
* onmouseup
* onmouover
* onmouseout
* onmousemove
结论：click = mousedown + mouseup
dblclick = click*2
```

### Event对象

#### 什么是event对象

​	监听事件执行过程中的状态，用来保存当前事件的信息对象

#### 如何获取event对象

```
ele.事件 = function(e){
	e = e || window.event;//获取event对象的兼容写法。IE8-：window.event
}
```

#### 鼠标event对象的属性

##### 1.鼠标按键

- button 返回当事件被触发时，哪个鼠标按钮被点击。
  - W3C标准
    0: 代表鼠标按下了左键
    1: 代表按下了滚轮
    2: 代表按下了右键
  - IE8-（IE8以下的浏览器）
    1鼠标左键， 2鼠标右键， 3左右同时按， 4滚轮， 5左键加滚轮， 6右键加滚轮， 7三个同时

###### 演示：检测鼠标按键

```
box.onmousedown = function(event){
    switch(event.button){
        case 0:
            box.innerText = '射击';
            break;
        case 1:
            box.innerText = '换枪';
            break;
        case 2:
            box.innerText = '开镜';
            break;
    }
}
```

##### 2.光标位置信息

```
clientX /clientY 光标相对于浏览器可视区域的位置，也就是浏览器坐标。
screenX/screenY 光标指针相对于电脑屏幕的水平/垂直坐标。
pageX/pageY:鼠标相对于文档的位置。
	* 包括滚动条滚动的距离，即：e.clientX+window.scrollX
	* IE8-不支持
offsetX,offsetY: 光标相对于事件源对象的相对偏移量。
	* 事件源对象：触发事件的对象
```

###### 演示:检测光标位置

###### 案例：新闻提示信息

	//1.给所有新闻绑定鼠标移入移出移动事件
	for(var i=0;i<links.length;i++){
	    links[i].onmouseover = function(){
	    	//2.移入事件：
	    		//2.1将title属性值作为details元素的内容
	       		//2.2显示details元素
	       		//2.3为了不让title默认行为影响，去除title属性
	            this.bak = this.title;  // 2.3.1移除前备份title内容,下一次移入才不会没有没有内容
	            this.removeAttribute('title'); // 2.3.2去除title属性
	    }
	    links[i].onmouseout = function(){
	        //3.移出事件：隐藏details。重置title属性值
	    }
	    links[i].onmousemove = function(e){
	    	//4.移动事件，设置定位
	    }
	}
#### event对象的键盘属性

- which/keyCode（ie浏览器）
  - 对于keypress事件，该属性声明了被敲击的键生成的 Unicode 字符码(ascii码)
  - 对于keydown和keyup事件，它指定了被敲击的键的虚拟键盘码。虚拟键盘码可能和使用的键盘的布局相关。
- altKey 当事件被触发时，ALT 键是否被按下。返回值为布尔值。
- ctrlKey 当事件被触发时，CTRL 键是否被按下。
- shiftKey 当事件被触发时，Shift 键是否被按下。

###### 案例：愤怒的小鸟

	//键盘按下，根据上下左右移动小鸟
	document.onkeydown = function(e){
		// 1.获取小鸟当前位置
	    var top = bird.offsetTop;
	    var left = bird.offsetLeft;
		//2.初始化速度及类名变量
	    var speed = 10;
	    var className = '';
	    // 3.判断按下上下左右方向键,设置left值及类名变量值
	    switch(e.keyCode){
	        case 37:
	            left -= speed;
	            className = '';
	            break;
	        //同理其他方向
	    }
	    // 4.通过变量值，设置bird的位置，及类名
	    bird.style.left = left + 'px';
	    bird.style.top = top + 'px';
		bird.className = className;
	}

### 事件冒泡

事件的执行阶段：事件冒泡、事件捕获、先捕获再冒泡

同一元素的同一事件只能在其中一个阶段执行

默认情况下，事件的执行都是默认在冒泡阶段执行。

##### 什么是事件冒泡：

> 在一个对象上触发某类事件（如onclick事件），那么click事件就会沿着DOM树向这个对象的父级传播，从里到外，直至它被处理程序处理，或者事件到达了最顶层（document/window）

###### 演示：从里到外的元素添加相同的事件，查看事件冒泡

1）不是所有的事件都能冒泡。

​	以下事件不冒泡：blur、focus、load、unload…。

​	【onmouseover与onmouseenter的区别】

 2）冒泡到最顶层的目标不同。大部分浏览器到window对象，IE8-到document对象 

##### 停止事件的传播、阻止冒泡

```
 标准：event.stopPropagation(); 
 IE8-：event.cancelBubble = true; 
 // 阻止事件冒泡兼容写法：
 if(e.stopPropagation){
 	e.stopPropagation();
 }else{
	e.cancelBubble = true;
 }
```

##### 事件委托

​	利用事件冒泡原理，把本来绑定给某个元素的事件委托给它的父级（已经存 在页面元素）处理。

###### 事件源对象：触发事件的元素

标准属性：target
IE8-属性：srcElement

###### 案例：表格删除当前行

```
//影响页面性能的三大操作：
	//* 事件数量
	//* dom节点操作次数
	//* 请求次数
output.onclick = function(e){	
	//兼容性问题
    e = e || window.event;
    var target = e.target || e.srcElement;
    if(target.className === 'btnDel'){
    	//this指的是谁?
        var currentTr = target.parentNode.parentNode;
        currentTr.parentNode.removeChild(currentTr);
    }else if(target.className === 'btnCopy'){
        var currentTr = target.parentNode.parentNode;
        currentTr.parentNode.appendChild(currentTr.cloneNode(true));
    }
}
```

### 阻止浏览器默认行为

> - 链接跳转
> - 表单提交
> - 右键菜单…
> - 文本的选择

- 标准：event.preventDefault();
- IE8-：event.returnValue = false;

###### 案例：自定义右键菜单

	document.oncontextmenu = function(e){
	    e = e || window.event;
	    //获取鼠标在文档中的位置
	    e.pageX = e.pageX || e.clientX + window.scrollX;
	    e.pageY = e.pageY || e.clientY + window.scrollY;
	    //给菜单框添加位置样式
	    contextMenu.style.left = e.pageX + 'px';
	    contextMenu.style.top = e.pageY + 'px';
	    contextMenu.style.display = 'block';
	    //阻止浏览器的默认行为
	    e.preventDefault ? e.preventDefault() : returnValue = false;
	}
	// 点击空白地方关闭右键菜单
	document.onclick = function(){
		contextMenu.style.display = 'none';
	}
	contextMenu.onclick = function(e){
		e = e || window.event;
		//禁止事件冒泡
		e.stopPropagation ? e.stopPropagation() : e.cancelBubble=true;
	}

### 事件捕获(反冒泡)

```
事件的传播：
（1）事件冒泡：从下往上 
（2）事件捕获：从上往下
		*监听器
```

###### 演示：事件捕获的效果

#### 事件监听器

```
//标准浏览器：元素.addEventListener(事件名,事件处理函数,是否捕获（默认false，为冒泡）)
target.addEventListener("click", fn, false);
//IE8-：元素.attachEvent(on+事件名,事件处理函数)
target.attachEvent("onclick",fun);
```

- 可以绑定多个处理函数在一个对象上, 执行顺序按照绑定的顺序来(标准)
  - 不同元素事件执行顺序跟html结构有关
  - 相同元素事件执行顺序跟绑定先后有关
- 可以绑定多个函数在一个对象上, 执行顺序按照绑定的反序（ie8-）

	##### 演示:绑定事件的两种方式的区别	

###### 封装：绑定事件，兼容浏览器

```
function bind(ele,type,handler,isCapture){
	// 优先使用事件监听器
	if(ele.addEventListerner){
		// 标准浏览器
		ele.addEventListerner(type,handler,isCapture);
	}else if(ele.attachEvent){
		// IE8-
		ele.attachEvent('on' + type,handler);
	}else{
		// DOM节点绑定方式
		ele['on' + type] = handler
	}
}
```

### 事件的移除

##### DOM绑定事件的移除

​	ele.on+事件 = null；

##### 事件监听器移除

- 标准：removeEventListener(type,fn, true)传入的参数fn要跟添加时一样(同一个函数)，否则不能移除事件
- IE8-：detachEvent('on'+type,fun)，传入的参数fun要跟添加时一样，否则不能移除事件

> 注意：
> 页面事件绑定数量越多，越影响性能（速度越慢）

### 拖拽效果

##### 拖拽的原理

​	鼠标按下且移动鼠标时，改变当前元素的top,left值

##### 拖拽思路及案例演示

	//给目标元素添加onmousedown事件
	//- 拖拽的前提是目标元素设置css定位
	//- 记录按下鼠标位置与元素左上角的偏移量offsetX，offsetY
	box.onmousedown = function(e){
		var ox = e.offsetX;
		var oy = e.offsetY;
	    //当onmousedown发生以后，此刻给document添加onmousemove事件
	    // - 意味着此刻鼠标在网页的移动都将改变目标元素的位置
	    // - 目标元素的left = 鼠标的pageX – ox
	    // - 目标元素的top = 鼠标的pageY– oy
	    document.onmousemove = function(e){
	        box.style.left = e.pageX - ox + 'px';
	        box.style.top =  e.pageY - oy + 'px';
	    }
	}
	//给目标元素或者document（效果有差异）添加onmouseup事件，清空document的onmousemove事件
	document.onmouseup = function(){
		document.onmousemove = null;
	}		

###### 案例：给图片实现拖拽

###### 案例：实现复杂的拖拽效果

    1.按拖拽原套路实现拖拽效果，唯一不同的在于初始鼠标位置与大盒子左上角初始偏移量获取
    2.用数组记录mousedown、mouseover时的大盒子的定位坐标
    3.mousedown，状态值改成true；mouseup，状态值改成false
    4.mousemove过程，改变坐标元素的值：dragTop.innerText
    h2.onmousedown = function(e){
    	dragStatus.innerText = 'true';
        pos.push({x:box.offsetLeft,y:box.offsetTop})
       	//注意点1：
        var ox = e.clientX - box.offsetLeft;
        var oy = e.clientY - box.offsetTop;
        document.onmousemove = function(evt){
            var left = evt.clientX - ox;
            var top = evt.clientY - oy;
            box.style.left = left + 'px';
            box.style.top =  top + 'px';
    		// 注意点2：取消文字选择
    		evt.preventDefault();
    		 pos.push({x:left,y:top});
            dragTop.innerText = top;
            dragLeft.innerText = left;
    	}
    }
    document.onmouseup = function(){
        document.onmousemove = null;
        dragStatus.innerText = 'false';
    }
    //5.点击轨迹回放，定时器拿到数组的每个索引的值，赋值给box的样式。
    //索引到达最后一个值的时候，清空定时器及数组
    box.onclick = function(e){
        e = e || window.event;
        var target = e.target || e.srcElement;
        if(target.tagName.toLowerCase() === 'a'){
            var i=pos.length;
            var timer = setInterval(function(){
                i--;
                box.style.left = pos[i].x + 'px';
                box.style.top =  pos[i].y + 'px';
                dragTop.innerText = pos[i].y;
                dragLeft.innerText = pos[i].x;
                if(i<=0){
                    clearInterval(timer);
                    pos = [];
                }
    	},50);
    }

###### 案例：表格编辑

	// 1.利用事件委托实现单元格编辑
	datalist.onclick = function(e){
	    e = e || window.event;
	    var target = e.target || e.srcElement;
	    //2.当点击td，进入编辑状态。
	    if(target.tagName.toLowerCase() === 'td'){
	    // 判断：第一列（target===target.parentNode.firstElementChild）、最后一列不进入编辑状态return
	        // （1）进入编辑状态：添加编辑类
	        target.className = 'active';
	        // （2）创建输入框，将当前td的内容给输入框
	        var input = document.createElement('input');
	        input.type = 'text';
	        input.value = target.innerText;
	        // （3）将td内容清空，给td插入input
	        target.innerText = '';
	        target.appendChild(input);
	        // （4）自动获得焦点
	        input.focus();
	    }
		// （5）失去焦点，保存内容
	    input.onblur = function(){
	    	target.innerHTML = input.value;
			// 推出编辑状态：清空编辑类
	        target.className = '';
	    }
	}

