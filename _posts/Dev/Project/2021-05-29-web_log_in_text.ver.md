---
layout: post
title: html & php웹페이지 로그인,회원 가입 기능 구현(텍스트 파일.ver)
categories: Project
message: 웹 프로그래밍
order: 1
---

## 웹페이지 로그인 기능 구현

기본적인 로그인,회원 가입, 회원 정보 수정 기능을 구현하였습니다.

__텍스트 파일.ver__ 은 사용자 정보를  _.txt_ 파일에 저장하고/읽는 방식

### index

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_text.ver-1.png">

__로그인 체크 기능__ 으로  사용자의 입력을 받아

회원 정보 텍스트 파일인 _studentinfo.txt_ 의 정보와 비교하여 로그인 기능을 수행.

```
## studentinfo.txt
JJU001||001A||AAAA||컴퓨터공학과||1||전주||JJU002||002B||BBBB||정보통신학과||2||광주||JJU003||003C||CCCC||건축학과||3||부산||JJU004||004D||DDDD||나노신소제공학과||4||정읍||JJU005||005E||EEEE||전기공학과||2||전주||
```



#### index.html

````html
<!DOCTYPE html>
<html>
<head>
<tilte>로그-인 실습</title>
</head>
<body>

<hr>
<form method=post action="index.php">
아 이 디   : <input type= "text" name = "id" size=10/> <br>
비밀번호  : <input type = "password" name ="pw" size=10/> <br>
<hr>
<input type="submit" value="로그- 인"> &nbsp;&nbsp;&nbsp;
<input type="reset" value="다시쓰기"> <hr>
</form>
<form>
<a href = './join.htm'><input type="button" value="회원가입">&nbsp;&nbsp;&nbsp;
</form>
</body>

</html>
````

​    

#### index.php

```php
<?php

extract(array_merge($_GET,$_POST));
$flag = 0;
$fptr1 = fopen("./studentinfo.txt", "r");

while (($flag ==0) && ($line1 = fread($fptr1, 1024))){
    $user = explode("||", $line1);
    $c = count($user)-1;
    $index = 0;
    
    for($i=0; $i<$c; $i += 6){
        if( !strcmp($id, $user[$i])) {
            if( !strcmp($pw, $user[$i+1])) {
                $flag = 1;
                $index = $i;
            }
        }
    }
    
}

if($flag == 1){
    print"{$user[$index]}  님의 로그-인을 환영합니다<br><hr>";
    print" 회원정보 <br><br>";
    
    print" 이름 :    {$user[$index+2]}  <br>";
    print" 학과 :    {$user[$index+3]}  <br>";
    print" 학년 :    {$user[$index+4]}  <br>";
    print" 거주지 : {$user[$index+5]}  <br><br>";

    print "<a href='./index.htm'><input type='button' value='로그아웃'></a>";
    print "<a href='./mod.htm'><input type='button' value='정보수정'></a>";
    print " <a href = './exit.htm'><input type='button' value='회원탈퇴'></a>";
}
else{	
    print "아이디와 패스워드를 다시 한번 확인해주세요<br><hr>";
    print "<a href = './index.htm'><input type='button' value='메인화면으로'></a>";
}
fclose($fptr1);
?>
```

### join

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_text.ver-2.png">

__회원 가입__ 기능으로 사용자에게 데이터를 입력 받아 _studentinfo.txt_ 파일에 저장하는 역할

#### join.html

```html
<!DOCTYPE html>
<html>
<head>
<tilte>회원가입</title>
</head>
<body>

<hr>
<form method=post action="./join.php">
아 이 디   : <input type= "text" name = "id" size=10> <br>
비밀번호  : <input type = "password" name ="pw" size=10> <br>
이름        : <input type= "text" name = "name" size=10> <br>
학과        : <input type= "text" name = "dept" size=10> <br>
학년        : <input type= "text" name = "grade" size=10> <br>
거 주 지   : <input type= "text" name = "region" size=20> <br>

<hr>
<input type="submit" value="가입신청"> &nbsp;&nbsp;&nbsp;
<input type="reset" value="다시쓰기"> <hr>
<a href= "./index.htm"><input type="button" value="이전화면"></a	>&nbsp;&nbsp;&nbsp;

</form>
</body>

</html>
```

​     

#### join.php

```php
<?php

extract(array_merge($_GET,$_POST));
$flag = 0;
$fptr1 = fopen("./studentinfo.txt", "r");

while (($flag ==0) && ($line1 = fread($fptr1, 1024))){
    $user = explode("||", $line1);
    $c = count($user)-1;

    for($i=0; $i<$c; $i += 6){
        
        if( !strcmp($id, $user[$i])) {
            $flag = 1;
        }
    }
}
fclose ($fptr1);

if($id == NULL || $pw == NULL || $name == NULL)				//id pw 이름 모두 입력
{
    print "아이디 비밀번호 이름은 필수입력정보입니다! <br>";
    print "<a href='./join.htm'><input type='button' value= '돌아가기'></a>";
}

else if($flag == 1){
    print "이미 사용중인아이디입니다 <br>";
    print "다른 아이디를 신청해주세요 <br>";
    print "<a href='./join.htm'><input type = 'button' value = '돌아가기'></a>";
}
else{
    $fptr1 = fopen("./studentinfo.txt", "a");
    $line1 =  "{$id}||{$pw}||{$name}||{$dept}||{$grade}||{$region}||";
    fwrite($fptr1, $line1, 1024);
    fclose($fptr1);
    print "회원가입완료<br>";
    print "회원가입을 환영합니다<br>";
    print "<a href='./index.htm'><input type='button' value='메인화면으로'></a>";
}




?>
```

### mod

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_text.ver-3.png">

__회원 정보 수정__ 으로 _studentinfo.txt_을 읽고 인증 과정을 거치고 해당 회원 정보를 수정한다.

#### mod.html

```html
<!DOCTYPE html>
<html>
<head>
<tilte>정보수정</title>
</head>
<body>

<hr>
<form method=post action="mod.php">
아 이 디   : <input type= "text" name = "id" size=10/> <br>
비밀번호  : <input type = "password" name ="pw" size=10/> <br>
이름        : <input type= "text" name = "name" size=10> <br>
학과        : <input type= "text" name = "dept" size=10> <br>
학년        : <input type= "text" name = "grade" size=10> <br>
거 주 지   : <input type= "text" name = "region" size=20> <br>
<input type="submit" value="입력"> &nbsp;&nbsp;&nbsp;

</form>

</body>

</html>
```

#### mod.php

```php

    <?php
    
    extract(array_merge($_GET,$_POST));					//회원정보 수정
    $flag = 1;							//로그인,회원가입과 다르게 falg=1부터시작
    $fptr1 = fopen("./studentinfo.txt", "r");
    
    while (($flag ==1) && ($line1 = fread($fptr1, 1024))){
        $user = explode("||", $line1);
        $c = count($user)-1;
	


        
        for($i=0; $i<$c; $i += 6){						// id와 pw가 같은지 검사
            if( !strcmp($id, $user[$i])) {
                if( !strcmp($pw, $user[$i+1])) {
                    $flag = 0;
                    $index = $i;
                }
            }
        }
        
    }
if($id == NULL || $pw == NULL)
{
    print "아이디 비밀번호 는 필수입력정보입니다! <br>";
    print "<a href='./join.htm'><input type='button' value= '돌아가기'></a>";
}
    
else  if($flag == 0){
        
$fptr2 = fopen("./studentinfo.txt", "w");

		
$new =  "{$id}||{$pw}||{$name}||{$dept}||{$grade}||{$region}||"; 						//수정할 정보
$old = "{$user[$index]}||{$user[$index+1]}||{$user[$index+2]}||{$user[$index+3]}||{$user[$index+4]}||{$user[$index+5]}||";   //기존정보


print"{$user[$index]}회원님의 정보가 변경되었습니다<hr>";
print "<a href = './index.htm'><input type='button' value='메인화면으로'></a>";


$mod = str_replace($old,"$new",$line1);						//기존정보에 수정할 정보로 바꿈
fwrite($fptr2, $mod);
fclose($fptr2);

    }
    else{
        print "아이디와 패스워드를 다시 한번 확인해주세요<br><hr>";
        print "<a href = './index.htm'><input type='button' value='메인화면으로'></a>";
    }
    fclose($fptr1);
    ?>
```

### exit

<img src="https://Froggy0m0.github.io/assets/img/Project/2021-05-29-web_log_in_text.ver-4.png">

__회원 탈퇴__ 기능 _studentinfo.txt_ 에서 해당하는 정보를 삭제한다.

#### exit.html

```html
<!DOCTYPE html>
<html>
<head>
<tilte>회원탈퇴!</title>
</head>
<body>

<hr>
<form method=post action="exit.php">

아 이 디   : <input type= "text" name = "id" size=10/> <br>
비밀번호  : <input type = "password" name ="pw" size=10/> <br>
<input type="submit" value="탈퇴!"> &nbsp;&nbsp;&nbsp;

</form>

</body>

</html>
```

#### exit.php

```php

    <?php
    
    extract(array_merge($_GET,$_POST));					//회원탈퇴
    $flag = 0;
    $fptr1 = fopen("./studentinfo.txt", "a+");
    
    while (($flag ==0) && ($line1 = fread($fptr1, 1024))){
        $user = explode("||", $line1);
        $c = count($user)-1;
        $index = 0;
$sp=0;				
$del=0;
        
        for($i=0; $i<$c; $i += 6){
            if( !strcmp($id, $user[$i])) {
                if( !strcmp($pw, $user[$i+1])) {
                    $flag = 1;
                    $index = $i;
                }
            }
        }
        
    }
if($id == NULL || $pw == NULL)
{
    print "아이디 비밀번호 이름은 필수입력정보입니다! <br>";
    print "<a href='./join.htm'><input type='button' value= '돌아가기'></a>";
}
    
else  if($flag == 1){
        
$fptr2 = fopen("./studentinfo.txt", "w");


for($i=0; $i<$index; $i++)
{
            $sp += mb_strlen($user[$i], "UTF-8")+2;			//배열에서 입력된 회원의 문자열이 시작되는 위치를 찾음
}
        
        for($i=0; $i<6; $i++)					//입력된 회원의 문자열의 길이를 찾음 (index ~ index +5 까지, ) 
        {
            $del += mb_strlen($user[$index+$i], "UTF-8")+2;
        }
print"{$user[$index]}회원님의 정보가 삭제되었습니다}<hr>";
 print "<a href = './index.htm'><input type='button' value='메인화면으로'></a>";
$txt= mb_substr($line1,$sp,$del);				//line1에서 sp위치부터 del까지를 추출해냄 
$remove = str_replace($txt,"",$line1);				//추출해낸 txt파일을 ""(공백)으로 바꿈
fwrite($fptr2, $remove);					//바꿔낸 결과값을 다시 파일에저장
fclose($fptr2);

    }
    else{
        print "아이디와 패스워드를 다시 한번 확인해주세요<br><hr>";
        print "<a href = './index.htm'><input type='button' value='메인화면으로'></a>";
    }
    fclose($fptr1);
    ?>
```

