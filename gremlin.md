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
이런 코드를 입력하게 되면 id가 참이 아니더라도 참인 값인 1=1과 or연산을 하게 되면 참이 되고 %23이라는 #(주석)을 사용해서 뒤에오는 query문을 주석 처리를 하게 되므로 query문이 참이 되어서 문제를 풀게 된다.

-----------------------
