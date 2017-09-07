# TROLL

## CODE
```php
<?php  
  include "./config.php"; 
  login_chk(); 
  dbconnect(); 
  if(preg_match('/\'/i', $_GET[id])) exit("No Hack ~_~");
  if(@ereg("admin",$_GET[id])) exit("HeHe");
  $query = "select id from prob_troll where id='{$_GET[id]}'";
  echo "<hr>query : <strong>{$query}</strong><hr><br>";
  $result = @mysql_fetch_array(mysql_query($query));
  if($result['id'] == 'admin') solve("troll");
  highlight_file(__FILE__);
?>
```

## solution
위 코드를 보면 id를 GET형식으로 받고 있지만, id에 admin이 들어가는 것을 걸러 내고 있다. 그러므로 url 뒤에 
```
?id=ADMIN
```
위 코드를 입력을 하게 되면 troll을 클리어 하게 된다. 이유는 ereg가 필터링을 하고는 있지만 대문자와 소문자를 구별하기 때문에 입력이 된것이다.

## ereg
```
ereg(찾을 문자열 , 대상 문자열)
```
위와 같은 방식으로 사용이되고, 대소문자를 구별한다.

--------------