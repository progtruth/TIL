# Web/Was Server 재기동 방법

## Web Server

`httpd.conf` 파일경로 : /engn/apache/conf/httpd.conf

설정 적용 후 웹서버 재기동

```
$ cd /engn/apache/bin> apachectl restart
```

서버기동 상태 확인

```
> ps -ef | grep httpd
```


## Was Server

tomcat재기동 sh파일(`startup.sh`)있는 dir로 이동

- `tbin` 명령어에 alias 적용해놓음.

```
$ tbin
$ ./shutdown.sh
```

tomcat 프로세스 종료되었는지 확인

```
$ ps -ef | grep catalina*
```

재기동

```
$ ./startup.sh
```

tomcat 프로세스 떠있는지 확인

```
$ ps -ef | grep catalina*
$ ps -ef | grep java | grep tomcat
```

