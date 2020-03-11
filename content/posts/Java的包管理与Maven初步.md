---
title: "Java的包管理与Maven初步"
date: 2020-03-10T16:37:38-07:00
draft: false
---
# 什么是包(package) 和 类路径

[维基百科](https://en.wikipedia.org/wiki/Java_package)中是这样介绍的，A Java package organizes Java classes into namespaces, providing a unique namespace for each type it contains. Classes in the same package can access each other's package-private and protected members. 我个人的理解就是Java将许一组类打包到一个单一的名称空间中，利用包实现对一个程序的拆封。

当然这里说的包管理的包是指 JAR 包。JAR 是一种压缩格式，可将多个 Java applet 及其所需组件（.class 文件、图像和声音）绑定到 JAR 文件中，即一堆字节码文件的集合。

类路径(classpath)相当于是`JVM`的指南针(？)，帮助`JVM`寻找类的字节码。


# 什么是包管理

这里是要稍微提一下`JVM`的使命了，简述来说可能就是按照类的全限定类名，加载、执行类的字节码。`JVM`通过ClassPath去指定的目录中查找相应字节码文件并运行。在这个过程中，`JVM`不会管你什么字节码是用来干什么的啊，或是这么运行符不符合当地法律啊，这些一律不care。只要你给`JVM`提供了ClassPath并且目标正确即可。

这就会引出一个问题，如果你引用的是第三方库，`JVM`该如何方便的找到这些相应的字节码文件？这个时候包的重要性就体现出来了。从相应的网站下载所需要的JAR包然后将其导入CLasspath中让`JVM`运行。

这个过程可能说起来很简单，但是运行在一个JAR包时，可能还需要另外一个JAR包中的一个字节码文件作为支撑。

**这就产生了包的依赖。**这种依赖代表着一个包在缺少它所依赖的另一个包时，`JVM`是无法运行的。同时，打个比方，A包依赖了B包，而B包这需要引用C包中的某个类，**这就产生了传递性依赖。**

# 传递性依赖带来的问题

传递性依赖这一现象使得我们需要将所有的JAR包添入到CLasspath中，这个过程可以说简单，也可以说是复杂。简单是因为你只需要下载下来相应的JAR包并且放到正确的位置即可，复杂则是因为你永远不知道他需要多少JAR包 ——— 每个JAR包背后站着多少个JAR包。

在Maven这一划时代的包管理出现前，程序猿大多是要手动写命令进行编译运行并且导入相应的包。直到Apache Ant的出现这种现象变得稍微简便了些，但还是要手动下载每个第三方类库，写 XML 配置，指定编译的源码目录、依赖的 jar 包和输出目录等。

Maven的出现打破了这一问题，当然包管理不只是Maven的全部。我们只需要将所需要的JAR包输入到`pom.xml`中即可，Maven会自动下载相应的依赖的包。所以我们不需要在自己管理Classpath了。他会在自己“阅读”完 jar 包后，前往本地或是线上仓库中下载相应的依赖包。

# 包冲突
当然，上述所描述的只是最理想的状态。Maven中有一个原则：**绝对不允许最终的 classpath 出现同名不同版本的 jar 包。**

Maven 解析 pom.xml 文件时，同一个 jar 包只会保留一个，那么面对多个版本的jar包，需要怎么解决呢？ 

1. Maven默认处理策略
   * 最短路径优先
        Maven 面对同一个包的不同版本时，会默认选择最短路径的那个 jar 包，即 `A->B->D1` 和 `E->D2` 中D2会被保留。
   * 最先声明优先
        如果路径一样的话，如： `A->B->C1`, `E->F->C2` ，两个依赖路径长度都是 2，那么就选择最先声明。


2. 移除某项依赖的依赖jar包
    手动在pom.xml中使用`<exclusion>`标签去排除冲突的jar包
```   
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        <version>1.4.4.RELEASE</version>
        <exclusions>
            <exclusion>
                <groupId>com.google.guava</groupId>
                <artifactId>guava</artifactId>
            </exclusion>
    </exclusions>
</dependency>
``` 
当然运用 `Maven helper` 插件中的 `Dependency Analyzer` 也是可以的。

# Maven 依赖中的 scope

scope 用于在不同代码目录中隔离第三方依赖，选项有compile/test/provided等：

* Maven 默认的依赖配置项中，scope 的默认值是compile（可以省略不写），会参与当前项目的编译，当然也包括测试，打包时候通常也需要包含进去，在main和test中都可见；
* test表示依赖项目仅仅参与测试相关的工作，包括测试代码的编译，典型的如junit，只在test层级中可见；
* provided表示只参与编译，不参与运行，使用 IDEA 尝试运行就能看到报错




