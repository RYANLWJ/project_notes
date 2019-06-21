# PHP 常用语法汇总

## 连接数据库

### conn.php

```
    <?php
    header('content-type:text/html;charset=utf-8');

    $servername = 'localhost'; //服务器名字
    $username = 'root'; //mamp用户名
    $password = 'root'; //密码
    $dbname = 'project'; //数据库名字

    //2.创建连接
    $connection = new mysqli($servername, $username, $password, $dbname);

    if ($connection->connect_error) {
        die("连接失败:" . $connection->connect_error); //如果连接失败会显示错误在页面,不显示则成功
    }

```

## 把多条数据变成数组

```
$datalist = array( //把前面拿的数据整理成数组
    'data' => $content,//list列表的数据
    'orderdata' => $content3,//此用户加入购物车的所有商品数据
    'currdata'=>$content4,//用户加入购物车当前页面商品的数据

);
//返回数据到前端
echo json_encode($datalist, JSON_UNESCAPED_UNICODE);
```

## 加入购物车简单逻辑

```
<?php
header('content-type:text/html;charset=utf-8');//设置编码

/*获取前端传过来的数据*/
$isok =isset($_POST['isok']) ? $_POST['isok'] : 'false';//默认false 避免浏览器刷新导致重复提交数据
$idname = isset($_POST['idname']) ? $_POST['idname'] : '';
$quan = isset($_POST['quan']) ? $_POST['quan'] : '';
$user = isset($_POST['username']) ? $_POST['username'] : '';
$del = isset($_POST['del']) ? $_POST['del'] : '';
//注意php中的关键词,像username,password这些;

include 'connection.php';//连接数据库

$sql = "SELECT * FROM jiankegoodslist WHERE id='$idname' ";//sql语句
$res = $connection->query($sql); //执行sql语句
$content = $res->fetch_all(MYSQLI_ASSOC); //拿数据

/*通过php拿相应数据*/
$goodsname = $content[0]['goodsname']; //商品名称
$img = $content[0]['img']; //商品图
$currprive = $content[0]['currprive']; //现价
$origprice = $content[0]['origprice']; //原价
$size = $content[0]['size']; //规格

$nowprice = substr($currprive,2,5);//中文占两个字节
//    $val = float($nowprice);
$total= $nowprice * $quan ;
$total =round($total,2);
// FIXME 保留两位小数还没做

$sql3 = "SELECT * FROM cartdata WHERE username = '$user'"; //查询订单数据库人的数据
$res3 = $connection->query($sql3); //执行查询语句
$content3 = $res3->fetch_all(MYSQLI_ASSOC); //拿数据

$sql4 = "SELECT * FROM cartdata WHERE idname = '$idname' AND username = '$user'";//查询当前使用者的订单
$res4 = $connection->query($sql4);//执行语句
$content4 = $res4->fetch_all(MYSQLI_ASSOC); //拿数据库里已有的数据

if($isok!='false'){//开关防止页面刷新导致重复把数据写入数据库
    if (!$res4->num_rows) {
        $sql2 = "INSERT INTO cartdata(goodsname,img,currprive,origprice,size,quan,total,username,idname) VALUES('$goodsname','$img','$currprive','$origprice','$size','$quan','$total','$user','$idname')";
        $res2 = $connection->query($sql2);
        $res4 = $connection->query($sql4);//执行语句
        $content4 = $res4->fetch_all(MYSQLI_ASSOC); //拿插入的第一条数据的数据
    }else{//如果用户已加入过这件商品到购物车则只更新,不插入,只更新

        $sql2 ="UPDATE cartdata SET quan = $quan+quan , total = $total+total WHERE idname = '$idname' AND username = '$user'";
        $res2 = $connection->query($sql2);
        $res4 = $connection->query($sql4);//执行语句
        $content4 = $res4->fetch_all(MYSQLI_ASSOC); //拿更新的最新的数据

    }
}

if($del=='yes'){
    // echo '122323';
    $sql5="DELETE FROM cartdata WHERE idname = '$idname' AND username = '$user'";
    $res5 = $connection->query($sql5);//执行删除语句
    $res4 = $connection->query($sql4);
    $content4 = $res4->fetch_all(MYSQLI_ASSOC); //拿更新的最新的数据
    $res3 = $connection->query($sql3); //执行查询语句
$content3 = $res3->fetch_all(MYSQLI_ASSOC); //拿数据
}

/*把前面拿的数据整理成数组*/
$datalist = array(
    'data' => $content,//list列表的数据
    'orderdata' => $content3,//此用户加入购物车的所有商品数据
    'currdata'=>$content4,//用户加入购物车当前页面商品的数据

);

/*把数据返回到前端*/
echo json_encode($datalist, JSON_UNESCAPED_UNICODE);

```
