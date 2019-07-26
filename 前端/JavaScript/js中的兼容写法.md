1  获取后一个兄弟节点的兼容写法

    var nextElement=oLi3.nextElementSibling||oLi3.nextSibling;
    
2. 获取前一个兄弟节点兼容写法
    
        var previousElement = oLi3.previousElementSibling || oLi3.previousSibling;


3. childNodes的兼容使用


            function getChildNodes(ele){
                //1.定义一个空的数组
                var arr=[];
                //2.获取指定元素
                var oEle=document.querySelector(ele);
                //3.将元素下边的所有儿子拿到
                for(var i=0;i<oEle.childNodes.length; i++){
                    //4.变量判断真的儿子(元素节点nodeType==1)
                    if(oEle.childNodes[i].nodeType==1){
                        //5.将真儿子添加到数组里
                        arr.push(oEle.childNodes[i])
                    }
                }
                //6.返回数组
                return arr;
            }
    
    
 4.拿到第一个儿子
 
     var No1 = oBox.firstElementChild || oBox.firstChild;
     
5.拿到最后 个儿子

     var oLast = oBox.lastElementChild || oBox.lastChild;
6.getElementsByClass的兼容性用法
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
7.js的浏览器识别写法
```
<!--[if lte IE 7]>
     <script>
         alert("小于ie9")
     </script>
<![endif]-->
```