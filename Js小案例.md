# JS 小案例

****

## 轮播图

### 1. Demo 1 （原生 JS)

```
/* html,css 部分 */
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">

    <title>Document</title>
    <link rel="stylesheet" href="../css/base.css">
    <style>
        #wrap {
            width: 700px;
            height: 400px;
            margin: 100px auto;
            position: relative;
        }

        #wrap .wrap-list {
            width: 100%;
            height: 100%;
            position: absolute;
            overflow: hidden;
            z-index: 1;
        }

        #wrap li {
            width: 100%;
            height: 100%;
            position: absolute;
            /* left: 500px; */
        }

        #wrap li img {
            width: 100%;
            height: 100%;
        }

        #wrap .posibtn {
            width: 100%;
            height: 100%;
            position: absolute;
            z-index: 2;
        }

        #wrap .posibtn .prev,
        #wrap .posibtn .next {
            width: 51.2px;
            height: 51.2px;
            border-radius: 25.6px;
            position: absolute;
            cursor: pointer;
            opacity: 1;
            transition: 0.5s;

        }

        #wrap .posibtn .prev {
            background: url(../images/arrow-left.png);
            background-size: contain;
            top: 180px;
            left: 10px;

        }

        #wrap .posibtn .next {
            background: url(../images/arrow-right.png);
            background-size: contain;
            top: 180px;
            right: 10px;

        }
        #wrap .posibtn .next:hover{
            background-color: rgba(255, 255, 255, 0.425);
        }
        #wrap .posibtn .prev:hover{
            background-color: rgba(255, 255, 255, 0.425);
        }

        #wrap .light {
            width: 100%;
            height: 20px;
            position: absolute;
            top: 360px;
            z-index: 3;
            left: 30px;
            text-align: center;
            left: 0;
        }

        #wrap .light span {
            cursor: pointer;
            margin-right: 5px;
            width: 19px;
            height: 5px;
            background-color: rgba(238, 238, 238, 0.644);
            display: inline-block;
            border-radius: 3px;

        }

        #wrap .light span.active {
            background-color: cornflowerblue;
        }
    </style>
    <script src="../js/commom.js"></script>
    <script>
        window.onload = function () {
        sideShow ('wrap',7,'active')
        }
    </script>
</head>

<body>
    <div id="wrap">
        <ul class="wrap-list">
            <!-- <li><img src="../images/lb1.jpg" alt=""></li> -->
        </ul>
        <p class="light">
            <!-- <span class="active"></span> -->
        </p>
        <!--上下按钮-->
        <div class="posibtn">
            <p class="prev"></p>
            <p class="next"></p>
        </div>
    </div>

</body>

</html>

/* js部分*/

/* NOTE 轮播图用法:
(爷父孙结构)
id传大盒子id名;
qa传图片数量;
图片名字应该写成lb[qa].jpg命名,自动生成;
cls传激活焦点图样式的class名 */
function sideShow(id, qa, cls) {
    var Wrap = getId(id);
    var Wrap_List = Wrap.firstElementChild;
    var PreBtn = getCn(Wrap, 'prev')[0];
    var NextBtn = getCn(Wrap, 'next')[0];
    var Light = getCn(Wrap, 'light')[0];

    var timmer = setInterval(next, 2000); //设置定时器
    var now = 0;
    var Imghtml = ''; //声明图片渲染变量
    for (var i = 1; i <= qa; i++) { //通过循环生成装在图片的元素
        Imghtml += '<li><img src="../images/lb' + i + '.jpg" alt=""></li>';
    }
    Wrap_List.innerHTML = Imghtml; //生成图片
    var lis = Wrap_List.children; //获得li的集合
    var iw = lis[0].offsetWidth; //这个获取lis集合里li的元素的宽度需要放在li被渲染出来的后面才能获取

    for (var li of lis) { //全部图先放在右边
        li.style.left = iw + 'px';
    }
    lis[0].style.left = 0; //把第一张放在可视区

    function next() {
        startMove(lis[now], {
            'left': -iw
        });
        now = ++now > lis.length - 1 ? 0 : now; //判断当前是第几张如果是最后一张了,就返回下标0
        lis[now].style.left = iw + 'px'; //前一张滑过之后,让第二张瞬间放到右侧开始,主要是为了实现下一次的循环
        startMove(lis[now], {
            'left': 0
        });
        lightClear();
    }

    function pre() {
        startMove(lis[now], {
            'left': iw
        });
        now = --now <= -1 ? lis.length - 1 : now;
        lis[now].style.left = -iw + 'px'; //把图片瞬间放在最左边
        startMove(lis[now], {
            'left': 0
        })
        lightClear();

    }
    Wrap.onmouseover = function () { //鼠标进入可视区,停止定时器
        clearInterval(timmer);
    }
    Wrap.onmouseout = function () { //鼠标离开可视区,开启定时器
        timmer = setInterval(next, 2000);
    }

    // PreBtn.onclick = function () { //点击上一张
    //     pre();
    //     lightClear();
    // }
    // NextBtn.onclick = function () { //点击下一张
    //     next();
    //     lightClear();
    // }

    var Lighthtml = ''; //准备生成焦点元素
    for (var j = 0; j < lis.length; j++) { //循环生成span
        Lighthtml += ' <span></span>';
    }
    Light.innerHTML = Lighthtml; //渲染焦点元素
    Light.firstElementChild.className = 'active'; //激活第一个焦点(默认)

    function lightClear() { //将焦点元素classname清除
        for (var j = 0; j < lis.length; j++) {
            Light.children[j].className = '';
        }
        Light.children[now].className = cls; //激活当前值的classname *(此处传参,不许引号)
    }

    for (let j = 0; j < Light.children.length; j++) {
        Light.children[j].onmouseenter = function () {
            if (j > now) { //如果点击的下标大于当前的画面下标,则先把当前的画面扫到左边,快速把当前应该出现的画面移到最右边
                startMove(lis[now], {
                    'left': -iw
                });
                lis[j].style.left = iw + 'px';

            } else if (j < now) { //如果点击的下标小于当前的画面下标,则先把当前的画面扫到右边,快速把当前应该出现的画面移到最左边
                startMove(lis[now], {
                    'left': iw
                });
                lis[j].style.left = -iw + 'px';

            }
            startMove(lis[j], {
                'left': 0
            })
            now = j; //把当前焦点下标值赋予给now
            lightClear(); //点亮焦点

        }
    }

    /* 防止多次点击造成过快切换功能 */

    var oldtime = new Date();
    NextBtn.onclick = function () {
        if (new Date() - oldtime >= 800) { //前后间隔时间限制
            next();
            lightClear();
        }
        oldtime = new Date();
    }
    PreBtn.onclick = function () { //点击上一张
        if (new Date() - oldtime >= 800) {
            pre();
            lightClear();
        }
        oldtime = new Date();
    }
}
```
