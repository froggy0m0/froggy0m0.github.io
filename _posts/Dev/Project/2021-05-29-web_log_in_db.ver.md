---
layout: post
title: html&php 웹페이지 성적 조회(DB.ver)
categories: Project
message: 웹 프로그래밍
order: 1
---

## 웹페이지 성적 조회 기능 구현

기본적인 로그인,성적 입력,성적 조회 기능을 구현하였습니다.

__DB.ver__ 은 사용자 정보를 _DB_ 에 구현하여  저장하고/읽는 방식

### 오류파일이있습니다....

> scoreinsert.php & scoreselect2.php 두 파일이 정상 작동 하지 않습니다.

__무슨 이유 때문 인지 db insert가 정상 작동을 하지 않습니다__.

__콘솔창으로 직접 데이터를 넣어두고 조회 기능이 정상 작동하고__ 

__코드가 if문에서 걸리지 않고 else문안에만 적용되는것으로보아 으로 보아__ 

__버전 차이의 문제보다는 sql 연결하는 과정의 코드 오류 인것같습니다.__ 

__scoreselect2.php의 기능은 원래 작동하지 않았습니다.__

### index

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_db.ver-1.png">

홈페이지의 메인 화면입니다.

### login-check.php

메인 메뉴의 로그인 버튼을 통해 login-check.php 연결되어 로그인 성공 여부를 판단합니다.

```php
<?php
session_start();
extract(array_merge($_GET,$_POST));


if ($id != "" && $pw != "") {
    $connect = mysqli_connect("localhost","root","Wjsansrk123@","students");
    $sql = "select * from info where id='$id' and pw = '$pw'";
    $result = mysqli_query($connect,$sql);
    
    $row = mysqli_fetch_array($result);
    
    if($id == $row['id'] || $pw == $row['pw'])
    {
        $_SESSION['id'] = $row[id];
        $_SESSION['name'] = $row[name];
        $_SESSION['dept'] = $row[dept];
        $_SESSION['tel'] = $row[tel];
        echo"<script>
	location = 'login-state01.php';
	</script>";
    }else{
        echo"<script>
	alert('정보가 올바르지않습니다! ');
	location = 'index.html';
	</script>";
    }
    
    mysqli_close($connect);
}else{
    echo"<script>
	alert('로그인해주세요! ');
	location = 'index.html';
	</script>";
}
?>
```



​     

### login-state01.php

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_db.ver-2.png">

사용자가 로그인에 성공하면 DB저장되는 사용자의 정보가 출력됩니다.

성적입력/성적변경 -> __score.php__

개인성적조회 -> __scoreselect.php__

전체성적조회 -> __scoreselect2.php__

### score.php

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_db.ver-3.png">

사용자가 성적을 입력하면 입력한 데이터 값이 __scoreinsert.php__에 의해 저장됩니다.

### scoreinsert.php

정상 작동하지 않는 코드입니다.

```php
<?php
session_start();
extract(array_merge($_GET,$_POST));

if($_SESSION['id'] == "")
{
    echo " <script>
alert('로그인이필요합니다');
location = 'index.html';
</script>";
}
else if($kor =="" or $eng =="" or $math =="")
{
    echo " <script>
alert('성적을 모두입력해주세요');
location = 'score.php';
</script>";
    
}
else if($kor <= 100 and $eng <= 100 and $math <= 100)
{
    
    $id = $_SESSION['id'];
    $total = $kor + $eng + $math;
    $avg = $total/3;
    
    $connect = mysqli_connect("localhost","root","12345","students");
    $sql1 = "select * from subject where id = '$id'";
    $result = mysqli_query($connect,$sql1);
    
    $row = mysqli_fetch_array($result);
    
    if($id == $row['id'])
    {
        $sql2 = "update subject set kor = '$kor', eng = '$eng', math = '$math' , total = '$total' , avg = '$avg' where id = '$id'";
        mysqli_query($connect,$sql2);
        echo " <script>
alert('정보변경이완료되었습니다');
location = 'login-state01.php';
</script>";
    }else{
        $sql3 = "insert into subject values('$id',$kor,$eng,$math,$total,$avg)";
        mysqli_query($connect,$sql3);    
        echo " <script>
alert('입력이완료되었습니다');
location = 'login-state01.php';
</script>";
    }
    

    
}
else
{
    
    echo " <script>
alert('성적입력 범위 0~100');
location = 'login-state01.php';
</script>";
    
}
?>

<img src="https://snaklad.github.io/assets/img/Project/2021-05-29-web_log_in_db.ver-4.png">

## scoreselect.php

​```php
<?php
session_start();
extract(array_merge($_GET,$_POST));

if($_SESSION['id'] == ""){
    echo " <script>
alert('로그인이필요합니다');
location = 'index.htm';
</script>";
}else{
    
    $id = $_SESSION['id'];
    $name = $_SESSION['name'];
    
    $connect = mysqli_connect("localhost","root","Wjsansrk123@","students");
    $sql1 = "select * from subject where id = '$id'";
    $result = mysqli_query($connect,$sql1);
    
    $row = mysqli_fetch_array($result);
    
    if($id != $row['id']){
        echo " <script>
alert('정보입력이필요합니다');
location = 'score.php';
</script>";
    }else{
    
    echo "$name 님의 성적 <br>";
    echo "국어 : $row[kor]<br>";
    echo "영어 : $row[eng]<br>";
    echo "수학 : $row[math]<br>";
    echo "총점 : $row[total]<br>";
    echo "평균 : $row[avg]<br>";
    }
    
}
?>
```

### scoreselect.php

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_db.ver-4.png">

```php
<?php
session_start();
extract(array_merge($_GET,$_POST));

if($_SESSION['id'] == ""){
    echo " <script>
alert('로그인이필요합니다');
location = 'index.htm';
</script>";
}else{
    
    $id = $_SESSION['id'];
    $name = $_SESSION['name'];
    
    $connect = mysqli_connect("localhost","root","Wjsansrk123@","students");
    $sql1 = "select * from subject where id = '$id'";
    $result = mysqli_query($connect,$sql1);
    
    $row = mysqli_fetch_array($result);
    
    if($id != $row['id']){
        echo " <script>
alert('정보입력이필요합니다');
location = 'score.php';
</script>";
    }else{
    
    echo "$name 님의 성적 <br>";
    echo "국어 : $row[kor]<br>";
    echo "영어 : $row[eng]<br>";
    echo "수학 : $row[math]<br>";
    echo "총점 : $row[total]<br>";
    echo "평균 : $row[avg]<br>";
    }
    
}
?>
```



### scoreselect2.php

정상 작동하지 않는 코드입니다.

```php
<?php
session_start();
extract(array_merge($_GET,$_POST));

$id =$_SESSION['id'];
$name =$_SESSION['name'];

$connect = mysqli_connect("localhost","root","Wjsansrk123@","students");
$sql = "select avg from subject order by 총점 asc";
$result =mysqli_query($connect,$sql);


if (mysqli_num_rows($result) > 0) {
    while($row = mysqli_fetch_assoc($result)) {
        echo "아이디: " . $row["id"]. " 나이:" . $row["age"]. "<br>";
    }
}
mysqli_close($connect); 
?>
```

## DB구조

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_db.ver-5.png">