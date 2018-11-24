---
title: Docs
layout: default
home: ../
---


# 入门指导

## 介绍

(http://www.hueniverse.com/hueniverse/2007/10/beginners-gui-1.html)是一个不错的入门指引，通过两个不同又有关联的服务说明了OAuth 1.0.
一个是照片分享的应用.另一个是照片打印的应用.在OAuth项目,照片分享应用是OAuth服务端,照片打印应用是OAuth客户端.

关于这个指导,我们会用OAuth和Spring Security在本地开发一个照片分享应用盒一个照片打印应用.我们将照片分享应用命名为"Sparklr",将照片打印应用命名为"Tonr".
一个叫"Marissa"的用户(在Sparkr和Tonr都有账号)会通过Tonr去获取她在Sparklr的照片,却不给Tonr认证信息.

现在有一个Sparklr应用分别使用OAuth 1.0和OAuth 2.0实现,Tonr也是.我们可以下载的代码
(https://github.com/spring-projects/spring-security-oauth)
然后参照下面文章的详细教程去运行应用
[samples/README.md](https://github.com/spring-projects/spring-security-oauth/tree/master/samples). 

OAuth 1.0|OAuth 2.0
---------|---------
Sparklr 1 | Sparklr 2
Tonr 1 | Tonr 2

每个应用都是一个标准的[Maven](http://maven.apache.org/) 工程,所以你要先安装Maven.每个应用都是集成了Spring Security的Spring MVC应用.如果你熟悉Spring和Spring
Security, 那你将会对那些配置文件很熟悉(OAuth2样例是一个用实例工程).

## 系统搭建

检出Sparklr和Tonr工程,然后大致看一下.尤其注意一下`src/main/webapp/WEB-INF`下的配置文件.
  
For Sparklr, you'll notice the definition of the OAuth provider mechanism and the consumer/client details along with the
[standard spring security configuration](http://docs.spring.io/spring-security/site/docs/4.0.x/reference/html/ns-config.html) elements.  For Tonr,
you'll notice the definition of the OAuth consumer/client mechanism and the resource details.  For more information about the necessary
components of an OAuth provider and consumer, see the [developers guide](devguide.html).

You'll also notice the Spring Security filter chain in `applicationContext.xml` and how it's configured for OAuth support.

### Deploy Sparklr

{% highlight text %}
    mvn install
    cd samples/oauth(2)/sparklr
    mvn tomcat7:run
{% endhighlight %}

Sparklr should be started on port 8080.  Go ahead and browse to [http://localhost:8080/sparklr](http://localhost:8080/sparklr). Note the basic
login page and the page that can be used to browse Marissa's photos. Logout to ensure Marissa's session is no longer valid.  (Of course,
the logout isn't mandatory; an active Sparklr session will simply bypass the step that prompts for Marissa's credentials before
confirming authorization for Marissa's protected resources.)

### Start Tonr.

Shutdown sparklr (it will be launched in the same container when tonr runs), then

{% highlight text %}
    mvn install
    cd samples/oauth(2)/tonr
    mvn tomcat7:run
{% endhighlight %}

Tonr should be started on port 8080.  Browse to [http://localhost:8080/tonr(2)](http://localhost:8080/tonr). Note Tonr's home page has a '2' on the end if it is the oauth2 version.

### Observe...

Now that you've got both applications deployed, you're ready to observe OAuth in action.

1. Login to Tonr.

   Marissa's credentials are already hardcoded into the login form.

2. Click to view Marissa's Sparklr photos.

   You will be redirected to the Sparklr site where you will be prompted for Marissa's credentials.

3. Login to Sparklr.

   Upon successful login, you will be prompted with a confirmation screen to authorize access to Tonr
   for Marissa's pictures.
    
4. Click "authorize".
  
   Upon authorization, you should be redirected back to Tonr where Marissa's Sparklr photos are displayed
   (presumably to be printed).

