
页面使用<applet></applet>标签
AppletServlet代码sevice方法中有一行是取session的数据，该行报空指针。


一个applet页面访问后台报空指针，定位发现该项目在本机tomcat容器中运行是正常的，打包部署在Websphere上也是正常运行，只有在weblogic容器上时会出现空指针。

通过fiddler拦截请求发现，部署在weblogic容器上的项目，截取到的信息里，获取不到cookie的JSESSIONID

分析数据后发现丢失的cookie在HTTP Only列都是Yes，查阅相关文档得知，weblogic.xml中关于session-descriptor的设置有一项参数为cookie-http-only，
默认值为true，表示该cookie只有Http请求时才会传递，而匹配条件设置界面中为Applet导致储存SessionID的Cookie未传到Servlet。



tomcat   
tomcat6支持配置HttpOnly, 该属性缺省是false, 可以在context.xml里配置



解决办法：
webloigc   
 通过在weblogic.xml中配置

xml代码
<session-descriptor>
       ……
        <cookie-http-only>false</cookie-http-only>
</session-descriptor>
