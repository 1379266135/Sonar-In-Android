# Sonar-In-Android
### Android集成SonarQube代码分析工具SonarQube工具应用在Android项目中，对于JDK和gradle版本有一定要求；
1 sonarqube-gradle-plugin 版本号 > 2.5；

2 要求jdk版本 >= 17；

3 gradle版本 >= 4.2；

4 如果项目中使用了低版本的jdk和gradle；例如 jdk 11， gradle 3.6.4；

### 可按如下步骤进行集成使用gradle在项目中集成SonarQube：

1 在项目根目录下的build.gradle文件中增加

```
buildscript {
		dependencies {
			......
			......
			classpath 'org.sonarsource.scanner.gradle:sonarqube-gradle-plugin:3.3'
		}
	}
```
	

2 在app 项目目录下的build.gradle文件中增加

```
//第一步
apply plugin: 'org.sonarqube'

//第二步
sonarqube {
    properties {
        //Sonar服务器地址
        property "sonar.host.url", "[sonar服务器地址]"
        //Token模式
        property "sonar.login","[token]"
        property "sonar.sourceEncoding", "UTF-8"
        property "sonar.projectKey", "AndroidChina"
        property "sonar.projectName", "AndroidChina"

        //需要扫描的上传检测代码的模块，可以选择也可以配置哪一些需要或者不需要上传的模块(这里指APP模块下面的java包里面的全部)
        property "sonar.sources", "src/main/java"
        property "sonar.projectVersion", project.version
        property "sonar.java.binaries", "[项目app目录路径]/build/intermediates/javac/a9999aDebug/classes"
        property "sonar.java.jdkHome", "[java_home路径]/Home"
    }
}
```


3 在项目gradle --> wrapper --> gradle-wrapper.properties文件中增加：

```
distributionUrl=https\://services.gradle.org/distributions/gradle-6.3-all.zip
```

4 在项目目录下的gradle.properties文件中修改 org.gradle.jvmargs 参数列表：

```
org.gradle.jvmargs=-Xmx4608M \
  -XX:MaxMetaspaceSize=2048M \
  -XX:+HeapDumpOnOutOfMemoryError \
  -Dfile.encoding=UTF-8 \
  --add-exports=java.base/sun.nio.ch=ALL-UNNAMED \
  --add-opens=java.base/java.lang=ALL-UNNAMED \
  --add-opens=java.base/java.lang.reflect=ALL-UNNAMED \
  --add-opens=java.base/java.io=ALL-UNNAMED \
  --add-exports=jdk.unsupported/sun.misc=ALL-UNNAMED \
  --add-exports=jdk.compiler/com.sun.tools.javac.tree=ALL-UNNAMED \
  --add-exports=jdk.compiler/com.sun.tools.javac.code=ALL-UNNAMED \
  --add-exports=jdk.compiler/com.sun.tools.javac.util=ALL-UNNAMED
```

5 在Android studio控制台中，执行如下命令进行扫描任务

```
// 第一步， 改变jdk版本；这个命令的意思是：后面将要执行的命令，所依赖的jdk版本是现在指定的版本
export JAVA_HOME=/Users/elaine/Library/Java/JavaVirtualMachines/corretto-17.0.9/Contents/Home

//第二步，执行sonarqube scanner命令
./gradlew sonarqube
```
