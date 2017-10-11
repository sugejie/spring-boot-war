# spring-boot-war
spring-boot打war包，并发布到tomcat

### 打包步骤：
* pom.xml修改打包类型为war
* 配置外部Tomcat<br>
在这里将scope属性设置为provided，这样在最终形成的WAR中不会包含这个JAR包，因为Tomcat或Jetty等服务器在运行时将会提供相关的API类。
```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-tomcat</artifactId>
	<scope>provided</scope>
</dependency>
```
* 注册启动类<br>
在src根目录创建ServletInitializer.java，继承SpringBootServletInitializer，覆盖configure()，把启动类Application注册进去。外部web应用服务器构建Web Application Context的时候，会把启动类添加进去。
```java
public class ServletInitializer extends SpringBootServletInitializer{
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(SpringBootWarApplication.class);
    }
}
```
* 打包
	* 方式一：双击IDEA右侧，Maven Projects -> Lifecycle -> package即可完成打包
	* 方式二：切换至项目根目录，执行maven打包命令，mvn clean package -Dmaven.test.skip=true
* 部署<br>
将war包拷贝至tomcat安装目录的webapp下，启动tomcat自动解压并部署<br>
`注意：关闭tomcat后才能将war包删除，若在启动时将war删除，关闭tomcat后解压后的项目也会删除。`


### 强制修改github自动识别的项目语言类型<br>
步骤<br>
	1）打开控制台，cd 到项目根目录，输入命令<br>
		`touch .gitattributes`<br>
	项目的根目录就多出了一个`.gitattribute`文件<br>
	2）用文本编辑器打开`.gitattribute`文件，输入以下内容<br>
		`*.js linguist-language=Java`  或者  `* linguist-language=Java`<br>
	意思就是将.js 或者 所有 文件当作Java语言来统计，简单粗暴。<br>
	3）将项目提交到Github上，此时项目的语言标签就变成了Java。<br>

