### 一、什么是数组
		
		数组，就是一组数据，数组是一个容器。JavaScript的数组，不限制类型；

### 二.数组的定义方式以下

方式一：
	
		var arr=[];//数组的字面量表达式;

方式二：

		var  arr=new Array();//数组的构造函数


方式三：

		var arr=Array();
### 三.数组的取值与赋值

    var arr=[];  
    arr[0]="周杰伦"；//数组的赋值 ["周杰伦"] 
    arr[0]="刘德华";//  ["周杰伦","刘德华"]

   数组的取值

    var arr=["蔡依林","张学友"];
    arr[0] ;// 通过下标取值；
##### 1.数组的长度

    数组有一个length 属性
    arr.length;

    
### 四.遍历数组: 

之前我们讲过通过下标获取单个数组元素, 但有时候我们需要批量使用数组, 这个时候我们需要遍历整个数组.

    1, 普通for循环
    for(var i=0; i<5; i++){
         console.log(arr[i]);
    }
    2, for...in遍历:  用于遍历数组或者对象
    for(var i in arr){
         console.log(arr[i]);
    }


### 五.数组一些常用方法

##### 1.数组的追加和删除

	push();//从数组最后一位追加元素，并返回数组的长度
    pop();// 删除数组的最后一位，并返回被删除的元素
    unshift() // 从数组的第一位，开始添加，并且返回新的长度
    shift() // 删除数组的第一位，返回被删除的元素
    
##### 2.特殊的指定删除某一个元素方式 

    delete arr[Index];//使用与typeof 相似哦
	
##### 3.数组的合并concat() 
   	
	var arr=[111,222,333];
    var arr1=["你好","她好"];
    arr.concat(arr1);//返回一个新的数组 [111,222,333,"你好","她好"]

##### 4.数组的链接join() （形成一个字符串）
   
    	var arr=[2017,8,14]; // join //不会改变原来的数组,返回一段指定格式的字符串
		arr.join("/");       // 返回一个字符串 "2017/8/14"
		
##### 5.数组的截取 slice与splice

1.slice 截取数组，但是不改变原先数组
    
	       var arr=[11,22,33,44];
           arr.slice(index,end);//index 起始位置 ,end 结束位置
2.splice,这个方法分为2个参数与多个参数，改变原数组
 		
    var arr=[11,22,33];
    arr.splice(1,1);// 22 从索引位置开始，删除后面的几位
第一个参数表示规定添加/删除项目开始的位置，使用负数可从数组结尾处规定位置

第二个参数表示要删除的项目数量。如果设置为 0，则不会删除项目。

第三个及后面的参数可选，表示向数组添加的新项目。

==splice的返回值为删除的项目==

    var arr=[11,22,33,44];
    arr.splice(1,0,"你好"); // [11,你好,22,33,44] 

    var arr=[11,22,33,44];
    arr.splice(1,1,"你好");// [11,你好,33,44] 
	
### 六、排序
##### 1.冒泡排除

   		var arr=[9,8,7,12,4,1,20,5];
         //外层减1，因为整个过程会少一轮的比较
 		for(var i=0;i<arr.length-1;i++){
			//每一轮都会少一个元素参与比较 所以 i,i就是轮次
			for(var j=0;j<arr.length-i-1;j++){
				//比较大小
			    if(arr[j]>arr[j+1]){
				//交换位置
				 var tmp=arr[j+1];
					arr[j+1]=arr[j];
					arr[j]=tmp;
				}
			}
		}

##### 2.选择排序 （打擂台）

		var arr=[9,8,7,12,4,1,20,5];
         //外层减1，因为整个过程会少一轮的比较
 		for(var i=0;i<arr.length-1;i++){
			//每一轮都会少一个元素参(找到最小的)与比较 所以 i,i就是轮次
			for(var j=i+1;j<arr.length-1;j++){
				//比较大小
			    if(arr[i]>arr[j]){
				//交换位置
				 var tmp=arr[i];
					arr[i]=arr[j];
					arr[j]=tmp;
				}
			}
		}

##### 3.快速排序
    // 取数组中间位数的数据，将然后将剩余数据跟中间位进行比较，小的放在左边，大的放在右边，然后用递归将左右采用同样的办法进行排序。
```
    var arr2=[23,12,24,53,122,452,213];
    function quickSort(arr) {
        if(arr.length<=1){return arr}
        var middleindex=Math.floor(arr.length/2);
        var middleVal=arr.splice(middleindex,1);
        var left=[];
        var right=[];
        //与中间数进行比较。
        for(var i=0;i<arr.length;i++){
            if(arr[i]<middleVal){
                left.push(arr[i]);
            }else{
                right.push(arr[i]);
            }
        }
        return quickSort(left).concat(middleVal).concat(quickSort(right));
    }
    arr3=quickSort(arr2);
```

##### 4.数组的排序方法 sort（）
   
    var arr=[33,44,11,22];
    arr.sort(function(num1,num2){
        return num1-num2;
    }) //[11,22,33,44]

	//整个方法是按照 ASCII码进行比较的。


##### 5.数组的倒序reverse（）
   	
    var arr=[33,44,11,22];
    arr.reverse();// [22,11,44,33]

  ==注意：该方法不是排序，仅仅做一个倒序==


      
