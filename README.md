# MFK（码法咖） - 一款简单易用的JAVA WEB框架
### 概述
    码法咖（MFK）是基于Filter实现的MVC框架。
* 配置简化  
框架摒弃了以往的xml配置，所有的配置均在一个Config类中实现。  
* 插件扩展  
框架采用插件化方式进行功能扩展，提供插件接口以供扩展，实现共享，框架默认内置了文件上传、数据源以及缓存插件。  
* 渲染器  
使用渲染器进行页面渲染，默认内置了五种渲染器，对外提供接口，可以定制个性化渲染器。  
* 拦截器  
提供类级别和方法级别的拦截器，拦截器可以方便的实现一些通用功能，比如登陆检验、参数检验、日志记录、功能调试等。
### 功能
* 支持mvc模式开发
* 支持文件上传
* 支持拦截器
* 支持多数据源配置切换
* 支持缓存
* 支持插件扩展
* 支持定制个性化渲染器
### 使用
### step 1
新建一个配置类AppConfig，此类需要继承Config接口，如下： 

    public class AppConfig extends Config {
      @Override
      public void configRoute(RouteManager routeManager) {

      }

      @Override
      public void configPlugin(PluginManager pluginManager) {

      }
    }
这里只是一个空实现，具体的配置会在后面步骤中介绍。  
### step 2
将框架添加到你的项目中。首先下载mfk-web.jar，将其添加到lib目录，然后在web.xml中添加如下配置：  

    <filter>
        <filter-name>LastFilter</filter-name>
        <filter-class>com.mfk.mvc.core.LastFilter</filter-class>
        <init-param>
            <param-name>MFKConfig</param-name>
            <param-value>com.test.config.AppConfig</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>LastFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
此处需要注意，尽量将此filter配置在其它filter之后，否则无法将整个项目纳入到框架中。   
### step 3
完成step 2就已经将框架纳入到项目中了，下面就可以开始开发之旅了。  
1. 首先新建一个UserController类，此类需要继承Controller类，用于业务逻辑处理，如下：  

        public class UserController extends Controller {
            public VoidRenderer getUser() {
                //取到前端传递的参数
                String userName = getParameter("userName");
                System.out.println("user is " + userName);
                return new VoidRenderer();
            }
        }

2. 然后在step 1中的AppConfig类中，添加如下配置：  

        @Override
        public void configRoute(RouteManager routeManager) {
            //此处进行访问地址配置
            routeManager.addRoute("/getUser", UserController.class, "getUser");
        }
### step 4  
至此，一个基本的功能就配置完毕了。  
发布程序，启动服务，在浏览器输入localhost:8080/getUser?userName=test即可见到后端控制台输出响应信息。
### 进阶
* 数据库增删改查
* 缓存的配置使用
* 拦截器的使用
### 示例
[Demo下载地址](https://github.com/mfk11/MFK)

