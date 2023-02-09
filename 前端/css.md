# css语法

<img src="C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230207204222202.png" alt="image-20230207204222202" style="zoom:80%;" />

# css选择器

![image-20230208143737158](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208143737158.png)

## css id选择器

![image-20230208144345828](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208144345828.png)

**唯一的      #**

## 类选择器

![image-20230208144508088](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208144508088.png)

![image-20230208144534733](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208144534733.png)

标签选择器<class选择器<id选择器

## 通用选择器

![image-20230208144641380](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208144641380.png)

执行效率最低，万不得已用

## 分组选择器

![image-20230208144843447](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208144843447.png)

例：

![image-20230208145125003](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208145125003.png)

![image-20230208145134050](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208145134050.png)

## 总结

![image-20230208145148762](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208145148762.png)

## 属性选择器

![image-20230208150003430](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208150003430.png)

​                                 ![image-20230208145802533](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208145802533.png)或者![image-20230208145820581](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208145820581.png)

 p[属性]



![image-20230208150917145](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208150917145.png)

![image-20230208150925347](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208150925347.png)

![image-20230208150933353](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208150933353.png)

## 层级选择器

![image-20230208151249464](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208151249464.png)

![image-20230208151301738](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208151301738.png)

表示p标签下的em标签，可以更多层，其他标签下的不会变；与分组选择器的差别是没有逗号

![image-20230208153522391](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208153522391.png)

https://www.w3school.com.cn/cssref/css_selectors.asp

*element*>*element* 选择器用于选取带有特定父元素的元素。

**注释：**如果元素不是父元素的直接子元素，则不会被选择。

![image-20230208154231666](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208154231666.png)

**实测貌似不能p>div，一般认为p不能嵌套div**。因为块元素不能嵌套在这些只能嵌套内联元素的标签里面

**兄弟元素选择器：**

以`+`将两个选择器分开，匹配所有作为指定元素的相邻同级的元素；当第二个元素`紧跟`第一个元素之后，且两个元素都属于同一个父级，则第二个元素被选中；

## active选择器

![image-20230208160204118](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208160204118.png)

![image-20230208160213711](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208160213711.png)

![image-20230208160334217](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208160334217.png)

# css样式

![image-20230208160545478](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208160545478.png)

# css注释

![image-20230208160620697](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208160620697.png)

# 颜色

![image-20230208160817366](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208160817366.png)

rgb红绿蓝

rgba(100,100,100,0.5)  a表示半透明，0为透明，1为完全不透明

hsla（，，，0.5）a表示透明度

# 背景图像

![image-20230208162059403](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208162059403.png)

# 边框

![image-20230208162137393](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208162137393.png)

两个即上下，左右。三个是上右左，没有的与对边对应。

border-width宽度

border-color颜色

![image-20230208162621926](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208162621926.png)

![image-20230208163215671](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208163215671.png)

圆角可以设置百分比，控制宽高可以变为正圆

![image-20230208163421828](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208163421828.png)

## 外边距

![image-20230208163536917](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208163536917.png)

![image-20230208164017742](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208164017742.png)

![image-20230208164006899](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208164006899.png)

![image-20230208163951191](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208163951191.png)

![image-20230208163932694](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208163932694.png)

### 外边距合并

![image-20230208164209827](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208164209827.png)

兄弟元素之间：

<img src="C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208164221434.png" alt="image-20230208164221434" style="zoom:67%;" />

包含元素之间：

<img src="C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208164323598.png" alt="image-20230208164323598" style="zoom:67%;" />

有空元素：

<img src="C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208164406536.png" alt="image-20230208164406536" style="zoom:67%;" />

为了打破吸收规律，可以在两个边框间加间隔，比如border-width

## 内边距

![image-20230208165729634](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208165729634.png)

# 高度和宽度

![image-20230208165907123](C:\Users\LHC\AppData\Roaming\Typora\typora-user-images\image-20230208165907123.png)