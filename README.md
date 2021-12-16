# spring-boot-memory-leaks

from: https://github.com/fourgates/spring-boot-memory-leaks

Example application that will cause a memory leak to be able to debug with [Visual VM](https://visualvm.github.io/).

Run the following command to build and start the application:

```
mvn package -DskipTests
java -Xms64m -Xmx128m -XX:MaxMetaspaceSize=128m -jar target/*.jar
```

then you can either `curl` or use your browser to hit a `threads` endpoint to create a thread related memory leak:

```
curl http://localhost:8080/threads

# or
curl http://localhost:8080/objects
```

or use [hey](https://github.com/rakyll/hey)

```
hey -c 10 -z 5s http://localhost:8080/objects
```

when found `java.lang.OutOfMemoryError: Java heap space`

```sh
$ jps
30640 Main
90832 Jps
39516 spring-boot-0.0.1-SNAPSHOT.jar

jcmd <pid> GC.heap_dump filename.hprof
```

then use  VisualVM > File > Load... [Open filename.hprof]
and view memory on tab [heapdump] filename.hprof

## Notes


`visualvm.bat`

```
@echo off

set JAVA_HOME=D:\Dev\Java\jdk8

start /min visualvm.exe --jdkhome %JAVA_HOME% --console suppress
```
