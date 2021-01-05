# Mobile-terminal-adaptation
探索移动端适配，记录学习过程中的细节点。 媒体查询 ，meta标签相关等

适配方案一：

 1.通过媒体查询
     它主要是通过查询设备的宽度来执行不同的css代码，最终达到界面的适配。

 2.媒体查询优势
     简单, 哪里不对改哪里
     调整屏幕宽度的时候不用刷新页面即可响应式展示
     特别适合对移动端和PC维护同一套代码的时候

 3.媒体查询劣势
     由于移动端和PC端维护同一套代码, 所以代码量比较大，维护不方便
     为了兼顾大屏幕或高清设备，会造成其他设备资源浪费，特别是加载图片资源

 4.应用场景
     对于比较简单(界面不复杂)的网页,可以考虑。
     对于比较复杂(界面复杂)的网页, 则没有使用的必要。

适配方案二：

    很多比较复杂的商城类不适合使用媒体查询进行适配。他们更倾向于去 开发两套代码。

    * 在PC端打开展示PC端页面
    * 在移动端打开展示移动端页面

    可以通过BOM的navigator对象中的userAgnet来获取代理头信息。

    封装一个方法用于判断打开移动端还是pc端页面，即可完成适配。

适配方案三：

  使用媒体查询结合rem方案，可以根据手机的不同尺寸进行适配。

1. 如何等比缩放?
  * 将设计图片等分为指定份数,求出每一份的大小
      例如: 750设计图片分为7.5份, 那么每一份的大小就是100px
  * 将目标屏幕也等分为指定份数,求出每一份的大小
     例如: 375屏幕也分为7.5份, 那么每一份的大小就是50px

  * 用 原始元素尺寸 / 原始图片每一份大小 * 目标屏幕每一份大小 = 等比缩放后的尺寸
     例如: 设计图片上有一个150，150的图片, 我想等比缩放显示到375屏幕上
     那么: 150 / 100 * 50 = 1.5*50 = 75px

2. 开发中使用计算公式?

  * 目标屏幕每一份的大小就是html的font-size: 50px
  * 使用时只需要用 "原始元素尺寸 / 原始图片每一份大小rem" 即可

      150 / 100 = 1.5 / 1.5rem

      1rem = 50px  / 1.5rem === 1.5*50 = 75px

3. 网站分析

  * 网易新闻
      ```
      750/100=7.5;

      375/7.5=50;

      320/7.5=42.7;
      ```
  * 苏宁易购
      ```
      750/50=15;

      375/15=25;

      320/15=21.33
      ```
适配方案四：

方案三中,根据媒体查询设置根元素（html），在手机尺寸非常多的情况下，需要写大量冗余的媒体查询。

我们可以通过js，动态的设置html元素的字体大小，简便我们的操作。


document.documentElement.style.fontSize = window.innerWidth / 7.5 + 'px';

适配方案五：设备像素比DPR

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>适配方案五</title>
    <script type="text/javascript">
        // console.log(window.devicePixelRatio);  // 物理像素 / css像素
        // 怎样缩小呢？
        // console.log(1.0 / window.devicePixelRatio);
        let scale = 1.0 / window.devicePixelRatio ;
        let text = `<meta name="viewport" content="width=device-width, initial-scale=${scale}, maximum-scale=${scale} minimum-scale=${scale} user-scalable=${scale}">`
        document.write(text); // 因为当前的script 就在head标签中所以可以直接用document.write
        document.documentElement.style.fontSize = window.innerWidth/ 7.5 + 'px';
    </script>
</head>
<body>
</body>
</html>
```