# Spring-Framework 源码环境搭建

1. 将github spring-framework同步到gitee 加速下载

2. 下载gradle https://gradle.org/releases/

   * 版本要匹配

     要根据gradle相应的版本选择合适的java版本，可以根据提示，有时候gradle会爆出不支持当前java版本，或提高gradle 版本或降低java版本等

   * idea 配置

     * gradle 的一些配置

       ![](https://gitee.com/dlulianzi/blogimage2/raw/master/img/20221105140908.png)

     * Windows环境要注意的一些问题

       1. 使用管理员权限运行idea 

          如果不是管理员权限，idea 运行Idea 会一直构建不了，立即失败，即没有权限。

       2. 关于windows defender

          1. 首先以管理员权限运行idea，不然没权限，而且就算没权限，报错也云里雾里。所以以管理员权限运行是第一步

          2. idea 有时候运行gradle 构建的时候，会提示访问失败，这时候要将windows defender 进行相应的排除。我是根据idea 提示进行排除的（因为以管理员权限运行，所以idea有权限处理这些问题，才提出相应的解决办法提示）。具体不清楚，遇到google即可。

3. 配置源

   1. build.gradle

      ```properties
      repositories {
          maven { url 'https://maven.aliyun.com/repository/public' } // 阿里云
          maven { url 'https://maven.aliyun.com/repository/gradle-plugin' } // 插件
          mavenCentral()
          maven { url "https://repo.spring.io/libs-spring-framework-build" }
      }
      ```

   2. settings.gradle

      ```properties
      pluginManagement {
      	repositories {
      		maven { url 'https://maven.aliyun.com/repository/public' } //阿里云
      		gradlePluginPortal()
      		maven { url 'https://repo.spring.io/plugins-release' }
      	}
      }
      ```

      

4. 构建项目

   1. gradle 构建

      1. 正常构建

         ```shell
         gradlew build
         ```

      2. 跳过测试构建

         ```shell
         gradlew build -x test
         ```

5. 遇到的其他问题

   1. win11 jdk版本变更

      如果本地电脑安装了多个jdk版本，要进行版本切换时，win11选择jdk是有顺序的，直接修改JAVA_HOME不一定生效，因为之前的jdk版本已经设置了javapath 放在%java_home%前面，所以直接设置的javapath更优先的.如

      ```properties
      C:\Program Files (x86)\Common Files\Oracle\Java\javapath;
      ```

      这是我们需要把%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;放到它的前面，才能生效我们需要的java版本