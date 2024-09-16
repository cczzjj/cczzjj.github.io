## Java

Java是一种广泛使用的计算机编程语言，拥有跨平台、面向对象、泛型编程的特性，广泛应用于企业级Web应用开发和移动应用开发

```bash
jdeps --print-module-deps .\libs\*.jar hellofx-1.0-SNAPSHOT-runnable.jar
```

```bash
jlink --module-path D:\projects\jdk-21.0.3+9\jmods --add-modules java.base,java.desktop,java.scripting,jdk.jfr,jdk.unsupported --output custom-jre
```