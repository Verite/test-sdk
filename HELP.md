## **一、引言**

Java sdk 怎么写，怎么使用

以下是 demo ，其中有两个项目：

**sdk-demo** ：用于提供 sdk。
**use-sdk-demo** ：用于使用提供的 sdk。
通过这个 demo 的学习，我们就可以学会 sdk 的使用方法

## **二、test-sdk**

这里我希望建立一个文件夹，该文件夹下有两个项目，一个项目是 sdk-demo，另一个项目是 **use-sdk-demo**，这样就可以使用 Idea 一下子打开两个项目进行管理。
一个文件夹下创建多个项目

------

首先我们先创建一个文件夹 **use-sdk-demo**，右键 Open Folder as IntelliJ IDEA Project 打开该文件夹，或者在 Idea 中，File -> Open 这个文件夹

打开 **use-sdk-demo** 文件夹后，右键 New -> Module，根据自己想要的模板创建项目，在 Location 中的 **use-sdk-demo** 后追加 sdk-demo 路径名称，Finish 即创建完了 **sdk-demo** 项目

右键 New -> Module，同样根据自己想要的模板创建项目，在 Location 中的 **use-sdk-demo** 后追加 **use-sdk-demo** 路径名称，Finish 即创建完了 **use-sdk-demo** 项目

创建 sdk-demo 项目

根据上述所说的步骤，在 **sdk-demo** 项目创建的时候，选择 Maven 模板创建项目，即可创建完我们所需的 **sdk-demo** 项目。

------

这里 Maven 为我们生成了默认的 pom.xml 文件：

```
	<groupId>com.example</groupId>
    <artifactId>sdk-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>sdk-demo</name>
    <description>sdk-demo</description>
```

写一个测试函数
创建一个 package，这是为了方便其他项目导入该类。

我们在 src/main/java 右键，New -> Package，这里按照个人命名的习惯命名为 *com.example.sdkdemo.persion.verite*。命名规则参考自：[Java 包名（package）命名规则](https://www.cnblogs.com/cht-/p/11968668.html)

接着我们就可以编写这个 TestSdk 主类了：

```java
package com.example.sdkdemo.persion.verite;

/**
 * @author Verite
 * @date 2023/3/22 11:52
 */
public class TestSdk {
    public static void main(String[] args) {
        TestSdk testSdk = new TestSdk();
        testSdk.greeting();
    }

    public void greeting() {
        System.out.println("Hello, I am a sdk-demo.");
    }
}
```

**导出 sdk**

现在我们开始导出 sdk 的 jar 包：

File -> Project Structure，选择 Artifacts，点击 +，选中 JAR -> From modules with dependencies…

在弹出来的窗体里，选中 sdk-demo 我们想要导出的 sdk 项目，Main Class 选中 persion.verite.TestSdk，JAR files from libraries 选择 copy to the output directory and link via manifest。

最后的 META-INF 的路径，需要手动编辑，修改为 sdk-demo 下的 resource 目录即可。最后点击 OK


这里需要选中 include in project build，点击 OK


Build -> Build Artifacts…，选中 sdk-demo:jar 点击 Build 即可在默认的 sdk-demo/out/artifacts/test_sdk_jar 目录下生成 sdk-demo.jar 包


这样，我们就生成了我们想要的 jar 包，可以在本地测试运行一下看看是否异常：

```
java -jar sdk-demo.jar
```

可以看到输出：

```

E:\workSpace\test-sdk\use-sdk-demo\lib>java -jar sdk-demo.jar
Hello, I am a sdk-demo.

```


既然 sdk-demo.jar 已经准备就绪，我们就开始在另一个项目中去使用它吧。

## **三、use-sdk-demo**

我们使用同样的 Maven 模板去创建 **use-sdk-demo** 项目，不同的是，我们需要在**use-sdk-demo** 下创建一个 lib 文件夹，用来存放 use-sdk-demo 文件。

我们将 **sdk-demo.jar** 拖放到 use-sdk-demo/lib 下，然后我们就可以编写代码了：

远程引用

```
<dependency>
    <groupId>com.example</groupId>
    <artifactId>sdk-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
```

本地文件引用

```
<dependency>
    <groupId>com.example</groupId>
    <artifactId>sdk-demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <scope>system</scope>
    <systemPath>E:/workSpace/test-sdk/use-sdk-demo/lib/sdk-demo.jar</systemPath>
</dependency>
```

```
package com.example.usesdkdemo.persion.verite;

import com.example.sdkdemo.persion.verite.TestSdk;

/**
 * @author Verite
 * @date 2023/3/22 13:22
 */
public class UseTestSdk {
    public static void main(String[] args) {
        TestSdk testSdk = new TestSdk();
        testSdk.greeting();
    }
}
```

运行代码可看到执行结果：

```
2023-03-22 13:23:13 JRebel:  #############################################################
2023-03-22 13:23:13 JRebel:  
Hello, I am a test sdk.
Disconnected from the target VM, address: '127.0.0.1:64089', transport: 'socket'
```


至此，完结，撒花：）

## **四、总结**

我们就可以在现在的项目中引入其他同事的 sdk 包。

