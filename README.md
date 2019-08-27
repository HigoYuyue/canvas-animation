### 简介
- H5新标签
- 矩形区域画布
- 使用 javascript 画图
- 可绘制路径、矩形、圆形、字符以及添加图像

### canvas 绘制基本说明
- 坐标系，左上角 0，0。X向右增大，Y向下增大

- 绘图就是使用JS调用Canvas的接口完成的，而这个接口就是Context（上下文）对象
- 使用[CanvasElement].getContext(‘2d’)来获取2D绘图上下文。`步骤1`
- 绘制起点：ctx.moveTo(x, y); //x,y 都是相对于 canvas盒子的最左上角。`步骤2`
- 绘制终点：ctx.lineTo(x, y); //从x,y的位置绘制一条直线到起点或者上一个线头点。`步骤3`
- 开始路径：ctx.beginPath();作用：将当前路径和之前路径隔离,方便你对当前路径进行设置样式,线宽.`步骤2(1)`
- 闭合路径：ctx.closePath();作用：闭合路径会自动把最后的线头和开始的线头连在一起。`步骤4`
- 描边：ctx.stroke();`步骤5`
- 填充：ctx.fill();`步骤5(1)`

### 画图
#### 线
```js
context.beginPath();//开始新的绘制路径
context.moveTo(0,500);//设置开始点
context.lineTo(500,0);//设置结束点并连接开始点和结束点
context.stroke();//将图像绘制出来
```
##### 基本属性
lineWidth
fillStyle = color //color可以是颜色值、渐变对象、图案对象
strokeStyle
##### 虚线
ctx.setLineDash([4,2])
ctx.lineDashOffset = 5

#### 表格
🌰棋盘格

#### 圆/矩形/弧线
```js
ctx.arc(x,y,r,sAngle,eAngle,counterclockwise);// counterclockwise true逆时针 false：顺时针(默认) 弧度和角度的转换公式： rad = deg*Math.PI/180; 
ctx.rect(x,y,width,height) //矩形
ctx.strokeRect(x, y, width, height);//立即stroke绘制即描边
ctx.fillRect(x, y, width, height);//立即填充
ctx.clearRect(x, y, width, hegiht);//清空rect
//画圆弧
ctx.moveTo(x0,y0);
ctx.arcTo(x1,y1,x2,y2,radius);//x0、y0指的是圆弧的起始点；x1、y1，x2、y2指的切线的两个点
```
圆形上面的点的坐标的计算公式:

x =x0 + Math.cos(rad) * R;//x0和y0是圆心点坐标
y =y0 + Math.sin(rad) * R;//注意都是弧度
#### 文字
```
ctx.font='30px Arial';
ctx.fillText('text',x,y);//实心文本
ctx.strokeText('text',x,y);//空心文本
```
#### 图片
```js
ctx.drawImage(image,x,y);//x,y左上角坐标
```
#### 画板
```js
//1.获取canvas和上下文
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');
    //2.监听鼠标的移动
canvas.onmousedown = function (event){
        //清屏操作
        ctx.clearRect(0,0,canvas.width,canvas.height);
        //开启路径
        ctx.beginPath();
        //起点
        ctx.moveTo(event.offsetX, event.offsetY);
        canvas.onmousemove = function (event){
            //终点
            ctx.lineTo(event.offsetX, event.offsetY);

            //设置颜色和线宽
            ctx.strokeStyle = 'red';
            ctx.lineWidth = 5;
            //绘制 描边
            ctx.stroke();
        };
        canvas.onmouseup = function (){
            canvas.onmousemove = null;
            canvas.onmouseup = null;
        };

    };
```
#### 圆环
🌰见非零正交原则示例
#### 饼状图
#### 帧动画
##### requestAnimationFrame
定义：浏览器用于定时循环操作的一个接口，类似于setTimeout，主要用途是按帧对网页进行重绘。
目的：
为了让各种网页动画效果（DOM动画、Canvas动画、SVG动画、WebGL动画）能够有一个统一的刷新机制，从而节省系统资源，提高系统性能，改善视觉效果
使用：
```js
function callback() {
  // Do whatever
  requestID=window.requestAnimationFrame(callback);//requestID 代表任务ID的整数值
}// 需要循环调用
requestID=window.requestAnimationFrame(callback);
```
取消重绘：
window.cancelAnimationFrame(requestID);
[了解更多](http://javascript.ruanyifeng.com/htmlapi/requestanimationframe.html)
#### 变形
##### 前提
ctx.save()
ctx.restore()
Canvas 的状态就是当前画面应用的所有样式和变形的一个快照。
Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就被推送到栈中保存。
可以调用任意多次 save 方法。每一次调用 restore 方法，上一个保存的状态就从栈中弹出，所有设定都恢复。
##### 方法
- translate(x, y) 移动 canvas 和它的原点到一个不同的位置。
- rotate(angle) 以原点为中心旋转 顺时针方向 以弧度为单位
- scale(x, y) x,y 分别是横轴和纵轴的缩放因子
- context.transform(a,b,c,d,e,f)
   
```
|a,c,e|
|b,d,f|
|0,0,1|
a,d代表水平、垂直缩放;b,c代表水平、垂直倾斜;e,f代表水平、垂直位移
transform() 方法的行为相对于由 rotate(), scale(), translate(), or transform() 完成的其他变换
ctx.setTransform(a,b,c,d,e,f) 方法，它不会相对于其他变换来发生行为。
```
#### 重复元素
被重复的元素可用于绘制/填充矩形、圆形或线条等等。
```js
context.createPattern(image/canvas,'repeat|repeat-x|repeat-y|no-repeat');// image 规定要使用的模式的图片、画布或视频元素
```
效果如下：
![27ea503b9fec4faa1b8a9c5fdd3038e6.png](evernotecid://32144DFA-56BA-424B-92B4-A6CC4E8E60A9/appyinxiangcom/24664158/ENResource/p55)


#### 样式相关
```js
context.fillStyle="red";// 填充色
context.strokeStyle = 'red';// 描边色
context.lineWidth = 0.5;// 设置线宽
context.shadowBlur=20 //阴影宽度
context.shadowColor = 'pink'//阴影颜色
context.shadowOffsetX //设置或返回阴影与形状的水平偏移距离
context.shadowOffsetY //设置或返回阴影与形状的垂直偏移距离
```
#### 渐变
| 线性渐变 | 径向/圆渐变 | 上色 |
|---|---|---|
| createLinearGradient(x1, y1, x2, y2) |createRadialGradient(x1, y1, r1, x2, y2, r2) |gradient.addColorStop(position, color)|
|起点 (x1,y1) 与终点 (x2,y2)|以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。|position 参数必须是一个 0.0 与 1.0 之间的数值，表示渐变中颜色所在的相对位置|
🌰
```js
//径向渐变
var lg=context.createRadialGradient(50,450,0,80,500,100);
lg.addColorStop(0,"#ff0000");
lg.addColorStop(0.5,"#00ff00");
//使用蓝色填充
lg.addColorStop(1,"#0000ff");
context.fillStyle=lg;
context.fillRect(0,400,200,200);
```
#### 填充——非零正交原则
交叉路径的填充问题，“非零环绕原则”，顺逆时针穿插次数决定是否填充。
使用一个初始值为0 的计数器判断该区域是否填充?

1. 对于路径中任意给定的区域,从该区域内部画一条足够长的线段,使此线段的终点完全落在路径范围外部.
2. 如果线段是与路径的顺时针部分相交，则计数器加1，
3. 如果线段是与路径的逆时针部分相交，则计数器减1
4. 若计数器的最终值不是0，那么此区域就在路径里面,在调用fill()方法时，浏览器就会对其进行填充
5. 如果计数器最终值是0，那么此区域就不在路径内部，浏览器也就不会对其进行填充
🌰圆环
```js
//1.获取canvas和上下文
var canvas = document.getElementById('canvas');
var ctx = canvas.getContext('2d');

//2.绘制圆环
//内圆
ctx.arc(200,200,100,0,2 * Math.PI,false);
//外圆
ctx.arc(200,200,200,0,2 * Math.PI,true);
//填充颜色
ctx.fillStyle = 'pink';
ctx.fill();
```

### 应用
- 可视化数据： 各类统计图表，比如:百度的echart
- 场景秀： 用Canvas实现动态的广告效果能够非常融洽的跨平台运行
- 游戏： canvas在基于Web的图像显示方面比Flash更加立体、更加精巧，canvas成为HTML5小游戏开发首选
- 其他可嵌入网站的内容 (多用于活动页面、特效)：类似图表、音频、视频，还有许多元素能够更好地与 Web 融合，并且不需要任何插件
### 注意事项
- 只有width和height两个属性，默认300*150，也可以使用css定义大小，但绘制时图像会伸缩以适应它的框架尺寸：如果CSS的尺寸与初始画布的比例不一致，它会出现扭曲。
- 同时存在描边和填充，需先填充后描边，避免覆盖
- canvas 中图形变换是叠加的。所以从第二次变换相对的不是初始状态。可以使用 save 和 restore 保存变换状态，使其中一个变换不影响其他变换。
- scale 变换会影响起始点坐标以及边框的宽度
### 其他
```js
// 度数转弧度 d2a(90)
function d2a(n){
            return Math.PI*n/180;
        }
```
更多可查看[canvas](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors)
### 踩坑指南
[坑](https://segmentfault.com/a/1190000012527772)

待调研
canvas 样式 宽高

save restore
requestAnimationFrame

beginPath
