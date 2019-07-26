### 一、DOM对象
    DOM就是Document  Object Model(文档对象模型)的缩写，DOM 是 W3C（万维网联盟）的标准。
      DOM是中立于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。

 	DOM 文档对象模型，用于操作文档的内容、结构和样式的核心对象，说白了就是操作页面的元素、属性、文本、事件等的核心模型。
 

### 二、DOM节点 
分为 属性节点、文本节点、元素节点
	
	nodeType==1 //表示 元素节点（重点记住）
	nodeType==2 //表示 属性节点（了解）
	nodeType==3 //表示 文本节点（了解）

	nodeValue //获取文本内容
	nodeName  //节点的名称

以上这些如何去得到了，是用于DOM对象里的一个属性，叫 childNodes 代码如下

    
    var oBox=document.getElementById("id");

 oBox.childNodes 是一个数组 

	for(var i=0;i<oBox.childNodes.length;i++){
		if(oBox.childNodes[i].nodeType==1){
		  //元素节点类型
		}
		if(oBox.childNodes[i].nodeType==3){
		  //文本节点类型
		}
	}
  是不是觉得奇怪，为什么没有 oBox.childNodes[i].nodeType==2,因为属性节点就是一个奇葩

<div id="oBox" style="background:yellow">我有点黄</div>
	<div id="oBox" style="background:yellow">我有点黄</div>

 	 oBox.attributes["style"].nodeType==2//true 这样就获取到了属性节点

尴尬吧，都已经使用attributes，足矣说明style是oBox的一个属性，再来查看nodeType==2，哈哈.. 老罗只想说，“你TM在逗我玩,欺负我不认识单词吗”。

DOM节点的属性
	学过CSS的人都认识这些，不在一一讲解。

	document.getElementById(‘box’).id;//获取 id 
    document.getElementById(‘box’).id ="person";//设置id
    document.getElementById(‘box’).style;//获取css样式对象 document.getElementById(‘box’).style.color;//获取 style 对象中 color 的值
    document.getElementById(‘box’).style.color=‘red’;//设置 style 对象中 color 的值
    document.getElementById(‘box’).className;//获取 class
    document.getElementById(‘box’).className=‘box’;//设置 class

### 三、元素节点
#### 1.获取元素的方式

   	document.getElementById("id");//通过ID获取DOM对象
    document.getElementsByTagName("div");//通过标签名称获取DOM对象
	document.getELementsByName("name");// 通过属性 name的值获取DOM对象

下面三个获取元素的方法在IE6、7、8下有兼容问题

	docuement.getElementsByClassName("类名称");//通过类名称获取DOM对象
    document.querySelector('.box>#name') 只拿第一个元素,括号内写法同css3
    document.querySelectorAll('')拿到符合条件的所有元素,括号内写法同css3
##### 1)自己封装的兼容ClassName方法，

docuement.getElementsByClassName("类名称");
在ie6、7、8下有兼容性问题，所以我们自己封装了一个类似的方法。
```
	    function getClassName(str) {
        var eleList=document.getElementsByTagName('*');    //获取页面所以标签
        var classArr=[];                                    //定义一个用来存储符合条件的类的数组
        for(var i=0;i<eleList.length;i++){                 //遍历元素
            if (eleList[i].className) {                      //判断元素是否有类名,有的进行下一步
                //遍历所有标签
                var classList= eleList[i].className.split(' ');     //将标签的类名用空格进行切割
                for(var j in classList){
                    if (classList[j]==str){                         //遍历元素i的所有类名,存在相同的时候将其推入classArr;
                        classArr.push(eleList[i])
                        break
                    }
                }
            }
        }
        return classArr
    }
```    
##### 2)获取父元素节点
使用parentElement方法获取父节点

    var parentNode=child.parentElement
##### 3)获取兄弟元素节点

获取后一个兄弟元素使用nextElementSibling方法(IE9以上及其他浏览器)或nextSibling方法(IE6、7、8) 
nextSibling在IE9或者其他浏览器中返回的是文本节点

    var nextNode = brother.nextElementSibling || brother.nextSibling;  //兼容性写法
获取前一个兄弟元素使用previousElementSibling方法(IE9以上及其他浏览器)或previousSibling方法(IE6、7、8) 

    var previousNode = brother.previousElementSibling || brother.previousSibling  //兼容性写法
    
##### 4)获取子元素节点
A.对父节点调用getElementsByTagName()或getElementByName()等方法;.

例如:

    <div id="box">
      <img src="images/1.jpg">
      <img src="images/2.jpg">
      <img src="images/3.jpg">
    </div>
    <img id="showImg" src="images/4.jpg" />

    //通过oBox元素节点, 获取其三个子元素节点img
    var oBox = document.getElementById("box");
    var aImg = oBox.getElementsByTagName("img");
B.对父使用children方法获取某元素的所有子元素节点(兼容性好)

    var aImg = oBox.children;
C.对父节点使用childNode方法

因为childNode方法会获取到换行的空白文本节点,所以使用时需要去除空白节点,我们可以定义一个如下函数:
```
function getChildNode(ele) {
            var arr=[]
            var eleNode=document.getElementById(ele);
           var childList= eleNode.childNodes
            for(var i=0;i<childList.length;i++){
                if (childList[i].nodeType==1){
                    arr.push(childList[i])
                }
            }
            return arr
        }
```

在非IE中, 标准的DOM具有识别空白文本节点的功能，而 IE自动忽略了, 如果要保持一致的子元素节点, 需要手工忽略掉它.
##### 5)获取第一个子元素和获取最后一个子元素节点

获取第一个子元素使用firstElementChild方法(IE9以上及其他浏览器)或firstChild方法(IE6、7、8) 

    var firstChild = parentNode.firstElementChild || parentNode.firstChild  //兼容性写法
    
获取最后一个子元素使用lastElementChild方法(IE9以上及其他浏览器)或lastChild方法(IE6、7、8)

    var lastChild = parentNode.lastElementChild || parentNode.lastChild     //兼容性写法
    
#### 2.添加元素节点
##### 1)添加或移动元素到尾部
方法一

使用createTextNode方法创建元素节点和文本节点,然后用appendChild方法分别将文本节点添加到div上和将div添加到body内部的最后面

appendChild添加的位置为元素内部的尾部,也可以用来移动元素到某元素内部的尾部.
```
var oDIV= document.createElement("div");
oDIV.className="box";
//4.创建文本节点(在内存中)
var oTxt=document.createTextNode("我是一个文本");
//5.把文件节点添加到 div节点里面
oDIV.appendChild(oTxt);
//6.把div添加到body里面
// 给父元素里面一个子元 (最后的位置)
document.body.appendChild(oDIV);
```
方法二

使用createTextNode方法创建元素节点,使用innerHTML方法为元素节点添加内容,然后用appendChild方法将div添加到body内部的最后面
```
var oDIV=document.createElement("div");
//4.给div.绑定 样式;
oDIV.className="box";
//5.在div.绑定文本内容
oDIV.innerHTML="我时候通过innerHTML添加文本";
//6.把div添加body的最后一个位置
document.body.appendChild(oDIV);
```
##### 2)添加或移动元素到任意位置
方法一

使用父.insertBefore(新元素,指定位置的子元素)向某一子元素的前方插入一个元素
```
//2.1 拿到div
var oDIV = document.querySelector("div");
//2.2 拿到ul标签
var oUl = document.querySelector("ul");
//2.3 现在指定的位置;
var position = oUl.children[0];
//3 往父元素里面添加 指定位置的 元素
oUl.insertBefore(oDIV, position);
```
由于没有insertAfter这个方法,所以如果需要插入一个元素到自定位置的后面,需要我们自己封装一个函数.
```
function myInsertAfter(parentNode, newElement, refElement){
var nextChild = refElement.nextElementSibling || refElement.nextSibling;
parentNode.insertBefore(newElement, nextChild)
}
```
方法二

使用insertAdjacentElement方法

    var obj=父元素.insertAdjacentElement(position, element);
    position:   'beforebegin': 在该元素本身的前面.
                'afterbegin':只在该元素当中, 在该元素第一个子孩子前面.
                'beforeend':只在该元素当中, 在该元素最后一个子孩子后面.
                'afterend': 在该元素本身的后面.
    element:要插入到DOM树中的元素
    返回值 : 插入的元素，插入失败则返回null.
    
==兼容性:IE9以下及Firefox48.0以下不支持==
    
#### 3.克隆(复制)元素节点
##### 1)浅克隆
浅克隆只复制被克隆者本身,不复制被复制者的后代节点及文本节点

    var oClone=oBox.cloneNode(false); //得到了一个oBox对象的副本
    document.body.appendChild(oClone);

##### 2)深克隆
深克隆不仅复制被克隆者本身,还复制他的文本节点及所有的后代节点

    var oClone=oBox.cloneNode(true);//得到了一个dom对象的副本
    document.body.appendChild(oClone);//追加到body里面
#### 4.替换元素节点
使用.replaceChild()方法

    父元素.replaceChild(替换元素，原元素)
    
#### 5.移除元素节点
方法一

父元素.removeChild(子元素),从父元素中移除子元素

    var oBox=document.querySelector('.box');
    var oInner=document.querySelector('.inner');//父盒子
       //从父元素中消失
    oInner.removeChild(oBox);
方法二

自杀模式,自己移除自身

    var oBox=document.querySelector('.box');
    oBox.remove();//自杀 ,在ie下有一点点 兼容性问题
    
#### 6.表格元素节点方法

    //创建表格行
    var newTr=oTab.insertRow();
    //创建表格行中的小格子
    var td1=newTr.insertCell()
    //表格对象里面有一个删除 行的方法,这个方法需要一个 指定 行数(tmpIndex)
    deleteRow(tmpIndex)
    
### 四、属性节点
#### 1, 属性节点的属性
attributes属性: 获取某元素节点的所有属性节点(返回一个数组)。
```
var box = document.getElementById(‘box’);
box.attributes;   //[object NamedNodeMap]
box.attributes.length;   //返回属性节点个数 
box.attributes[0];   //[object Attr], 返回第一个属性节点 

box.attributes[0].nodeType;    //2，节点类型 
box.attributes[0].nodeName;  //属性名称 
box.attributes[0].nodeValue;   //属性值 

box.attributes[‘id’];   //返回属性名称为id的节点
box.attributes.getNamedItem(‘id’);   //返回属性名称为id的节点

```
#### 2，属性节点操作的三个函数
我们通过三个函数可以操作元素的属性:

getAttribute(): 通过属性名获取对应的属性值 
    
    //可以用来获取无法使用.点语法的自定义属性值。
    document.getElementById(‘box’).getAttribute(‘class’); 获取元素的class值
    
setAttribute(): 设置一个key-value形式的属性键值对 

    //setAttribute()方法可以给元素添加属性; 如果属性已经存在, 则会覆盖原来的属性;  需要传入两个参数：属性名和属性值.
    document.getElementById(‘box’).setAttribute(‘align’, ‘center’); 

removeAttribute(): 移除指定的属性

    //移除属性
    document.getElementById('box').removeAttribute('style');  
    
#### 3，获取style样式
```
function getStyle(obj, attr){
        // 非ie,google,火狐
        if(window.getComputedStyle){
            return window.getComputedStyle(obj, null)[attr];
        }
        //ie 6 8 9
        return obj.currentStyle[attr];
    }

```
getComputedStyle 返回的对象是 CSSStyleDeclaration 类型的对象。取数据的时候可以直接按照属性的取法去取数据，

例如 style.backgroundColor。需要注意的是，返回的对象的键名是 css 的驼峰式写法，background-color -> backgroundColor。

**和 style 的不同**

element.style 读取的只是元素的内联样式，即写在元素的 style 属性上的样式；而 getComputedStyle 读取的样式是最终样式，包括了内联样式、嵌入样式和外部样式。

element.style 既支持读也支持写，我们通过 element.style 即可改写元素的样式。而 getComputedStyle 仅支持读并不支持写入。

我们可以通过使用 getComputedStyle 读取样式，通过 element.style 修改样式。
### 五、innerHTML与innerText的区别

	innerHTML="123<input type='text'>" //设置值
	innerHTML //没有等于符号，表示获取 文本与html代码，返回的是字符串

	innerText 作用同上，只不过只能获取文本内容，不会获取html代码结构
	
==innerText在火狐的40几的版本有兼容问题==

### 六、HTML5新标签在老版本浏览器的兼容
html5的新标签在ie 6 7 8下有兼容问题，可以使用动态创建标签的方法来解决
    
    document.createElement("header");
也可以使用html5shiv 组件解决

    <script src="html5shiv/html5shiv.js"></script>
    <script src="html5shiv/html5shiv-printshiv.js"></script>
### 五、this
[彻底弄懂js中的this指向](https://blog.csdn.net/qq_33988065/article/details/68957806)









