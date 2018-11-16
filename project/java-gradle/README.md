[![official JetBrains project](http://jb.gg/badges/official.svg)](https://confluence.jetbrains.com/display/ALL/JetBrains+on+GitHub)

# How to launch tests
## JTReg tests

Before launching _jtreg_-tests the following parameters have to be specified in `gradle.properties` file:

* `stable_jdk` - path to stable JDK, it is used to run the harness _jtreg_. If this parameter is not specified, then
the harness is launched on JDK, provided by `JAVA_HOME`. Otherwise `StopExecutionException` is thrown.
<br>Note _MacOSX_: If neither `stable_jdk` nor `JAVA_HOME` are specified then stable JDK is defined by invoking
```/usr/libexec/java_home -V 1.8```

* `test_jdk` - path to JDK for the tests. If it is not specified then all tests are executed on locally built JDK. Or,
if there are no local JDK images, `JAVA_HOME` is used. Otherwise `StopExecutionException` is thrown.

* `jtreg` - path to the directory containing the _jtreg_ harness. This directory can be defined via the `JT_HOME`
environment variable as well. In case if location of the harness is not specified `StopExecutionException` is thrown.  

* `javaoptions` - additional options to JVM for running tests. This option can be omitted. Here is an example how this
option can be defined in `gradle.properties`:
```
javaoptions=\
  -Djava.awt.headless=false \
  -Xms128m \
  -Xmx750m \
  -XX:ReservedCodeCacheSize=240m \
  -XX:+UseCompressedOops
``` 

* `testlist` - collections of tests to be executed. If it is not specified, then all tests are executed
```
 testlist=\
  java/awt/Focus/ClearGlobalFocusOwnerTest/ClearGlobalFocusOwnerTest.java \
  java/awt/event/KeyEvent \
  javax/swing
```

In order to launch _jtreg_-tests do the following (in this case all required properties are specified in
`gradle.properties` file):
```
$ ./gradlew runjtreg
```
or specifying properties via command line
```
$ ./gradlew runjtreg -Pstable_jdk=/Library/Java/JavaVirtualMachines/jdk-10.0.2.jdk/Contents/Home \
 -Pjavaoptions="-Djava.awt.headless=false -Xms128m" \
 -Ptestlist="java/awt/Focus/ClearGlobalFocusOwnerTest java/awt/event/KeyEvent"
 ```

 ## JBRE JUnit tests

In order to launch _JBRE JUnit_-tests do the following:
```
$ ./gradlew test --tests  quality.text.DroidFontTest --tests quality.text.MixedTextTest \
--tests quality.text.TextMetricsTest -Ptest_jdk=../../%jdk.under.test%
 ```