# 飘窗效果


## 第一种实现

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>浮窗效果</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        #box {
            width: 200px;
            height: 200px;
            left: 0;
            top: 0;
            border: 1px solid #eee;
            box-shadow: 0 0 5px #ccc;
            position: absolute;
        }

        #box h2 {
            padding: 8px 4px;
            font-size: 16px;
            color: #666666;
        }

        #box p {
            padding: 8px 4px;
            font-size: 12px;
            color: #a9a9a9;
            line-height: 20px;
        }
    </style>
</head>
<body>
<div id="box">
    <img src="octocat.png" alt="章鱼猫"/>
    <p><a href="#">章鱼猫</a></p>
</div>
<script>
    let box = document.getElementById('box');
    let speedX = 15, speedY = 12;
    //水平方向运动最大值
    let
        maxL = document.documentElement.clientWidth - box.offsetWidth;
    maxT = document.documentElement.clientHeight - box.offsetHeight;
    let timer = null;
    box.onmouseenter = function () {
        clearInterval(timer);
    };
    box.onmouseleave = function () {
        autoMove();
    };
    autoMove();

    function autoMove() {
        timer = setInterval(() => {
            let
                nextX = box.offsetLeft + speedX;
            nextY = box.offsetTop + speedY;

            //左侧边界
            if (nextX <= 0) {
                nextX = 0;
                speedX *= -1;
            }
            //上侧边界
            if (nextY <= 0) {
                nextY = 0;
                speedY *= -1;
            }
            //右侧边界
            if (nextX >= maxL) {
                nextX = maxL;
                speedX *= -1;
            }
            //底侧边界
            if (nextY >= maxT) {
                nextY = maxT;
                speedY *= -1;
            }
            box.style.left = nextX + 'px';
            box.style.top = nextY + 'px';
        }, 80);
    }
</script>
</body>
</html>
```

## 第二种实现，更平滑

``` html
<!DOCTYPE HTML>
<html>
<head>
    <meta charset="utf-8">
    <title>飘窗效果</title>
    <style type="text/css">
        * {
            margin: 0px;
            padding: 0px
        }

        #ad {
            position: absolute;
            left: 0px;
            top: 0px;
        }
    </style>
</head>
<body>
<div id="ad"><img src="octocat.png" alt="章鱼猫"/>
    <p><a href="#">章鱼猫</a></p></div>

</body>

<script type="text/javascript">
    //通过ID获取img
    ad = document.getElementById("ad");
    //定义横纵坐标
    x = 0;
    y = 0;
    //设置初始速度
    xv = 1.5;
    yv = 1.5;

    //根据img横纵坐标位置，设置反方向速度
    function fun() {
        if (x < 0 || x > window.innerWidth - ad.offsetWidth) {
            xv = -xv;
        }
        if (y < 0 || y > window.innerHeight - ad.offsetHeight) {
            yv = -yv;
        }
        x += xv;
        y += yv;
//将xy坐标值赋予img css样式中的left与top
        ad.style.left = x + "px";
        ad.style.top = y + "px";
    }

    //定时器调用
    mytime = setInterval(fun, 100);
    //ad绑定鼠标悬停事件
    ad.onmouseover = function () {
//清除定时器
        clearInterval(mytime);
    }
    //鼠标离开，重新触发定时器
    ad.onmouseout = function () {
        mytime = setInterval(fun, 100);
    }</script>
</html>

```


