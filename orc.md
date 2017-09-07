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