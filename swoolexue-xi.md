

# 4-1-创建TCP服务器

```php
<?php

//创建Server对象，监听 127.0.0.1:9501端口
$serv = new swoole_server("127.0.0.1", 9501);

//swoole_server->set函数用于设置swoole_server运行时的各项参数
$serv->set([
    'worker_num' => 4 , // worker进程数，cpu 1-4倍
    'max_request' => 10000,//最大进程数
]);
/**
 * 监听连接进入事件
 * $fd 客户端连接的唯一标示
 * $reactor_id 线程id
 */
$serv->on('connect', function ($serv, $fd, $reactor_id) {
    echo "Client: {$reactor_id} - {$fd}-Connect.\n";
});
/**
 * 监听数据接收事件
 * $reactor_id = $from_id
 */
$serv->on('receive', function ($serv, $fd, $reactor_id, $data) {
    $serv->send($fd, "Server: {$reactor_id} - {$fd}".$data);
});
//监听连接关闭事件
$serv->on('close', function ($serv, $fd) {
    echo "Client: Close.\n";
});
//启动服务器
$serv->start();
```

`<?php`

`//创建Server对象，监听 127.0.0.1:9501端口`

`$serv = new swoole_server("127.0.0.1", 9501);`



`//swoole_server->set函数用于设置swoole_server运行时的各项参数`

`$serv->set([`

`    'worker_num' => 4 , // worker进程数，cpu 1-4倍`

`    'max_request' => 10000,//最大进程数`

`]);`

`/**`

` * 监听连接进入事件`

` * $fd 客户端连接的唯一标示`

` * $reactor_id 线程id`

` */`

`$serv->on('connect', function ($serv, $fd, $reactor_id) {`

`    echo "Client: {$reactor_id} - {$fd}-Connect.\n";`

`});`

`/**`

` * 监听数据接收事件`

` * $reactor_id = $from_id`

` */`

`$serv->on('receive', function ($serv, $fd, $reactor_id, $data) {`

`    $serv->send($fd, "Server: {$reactor_id} - {$fd}".$data);`

`});`

`//监听连接关闭事件`

`$serv->on('close', function ($serv, $fd) {`

`    echo "Client: Close.\n";`

`});`

`//启动服务器`

`$serv->start();`

\`\`\`





\#\#\#\# 进入到代码位置



\`\`\`

-rwxrwxrwx. 1 root root 1354 6月  18 17:22 http\_server.php

-rwxrwxrwx. 1 root root  953 6月  16 22:18 tcp.php

-rwxrwxrwx. 1 root root  652 6月  18 19:25 ws\_server.php

\[root@hadoop1 server\]\# php -v

PHP 7.2.6 \(cli\) \(built: Jun  5 2018 00:45:24\) \( NTS \)

Copyright \(c\) 1997-2018 The PHP Group

Zend Engine v3.2.0, Copyright \(c\) 1998-2018 Zend Technologies

\[root@hadoop1 server\]\# php tcp.php 



\`\`\`



\#\#\#\#\# 发现问题，PHP -v环境不存在

&gt; 解决办法：进入PHP文件位置  /home/zws/study/soft/php，执行 source \`/.bash\_profile



\#\#\# 测试方式

\#\#\#\# 1、再次测试tcp.php文件，新开窗口会看到9501进程

\`\`\`

\[root@hadoop1 ~\]\# netstat -anp \| grep 9501

tcp        0      0 127.0.0.1:9501              0.0.0.0:\*                   LISTEN      81394/php     

\`\`\`

\#\#\#\# 2、新开窗口连接测试

\`\`\`

\[root@hadoop1 ~\]\# telnet 127.0.0.1 9501

Trying 127.0.0.1...

Connected to 127.0.0.1.

Escape character is '^\]'.



能看到主窗口

\[root@hadoop1 server\]\# php tcp.php 

^CClient: 0 - 1-Connect.



在窗口输入344，客户端能收到信息

\[root@hadoop1 ~\]\# telnet 127.0.0.1 9501

Trying 127.0.0.1...

Connected to 127.0.0.1.

Escape character is '^\]'.

344

Server: 0 - 1344



\`\`\`





