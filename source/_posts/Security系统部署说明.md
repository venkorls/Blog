---
title: security系统部署说明书
---

security系统部署说明书
<!--more-->

#   security系统部署说明书

# 架构关系

security 与各需要安全集成的应用是server/client的关系。

security是一个web应用，对外提供一个客户端jar和一组API。

security 是一个标准web应用组件，需要在web应用服务器中运行。

# 安装步骤

1.war包配置

	通过解压软件，打开war包，按“配置说明”中的示例进行配置。

```
a. WEB-INF/classes/bonc-security.properties

该配置文件下, 按照项目需求准确配置.
```

    #web系统名称
    web.name=安全系统
    #口令强度(low-低强度,大写,小写,字符,数字包含两种,长度>=6,common-标准,大写,小写,字符,数字包含三种,长度>=8,  high-高强度,大写,小写,字符,数字包含四种,长度>=12,默认是common)
    password.strength=common
    
    #加密类型(MD5/b64_sha1)
    #password.encrypt.type=MD5
    
    #找回密码方式，phone/email/all(用户自由选择)
    forgetPwd.retrieveType=all
    
    #忘记密码短信服务(提供发送短信接口的rest服务)
    phone.service=http://127.0.0.1:8080/smsinterface/sms/headquarters/sendlist
    
    #忘记密码，邮箱服务器
    mail.smtp.host=mail.bonc.com.cn
    #忘记密码，邮件源账
    mail.form=xxx@bonc.com.cn
    #忘记密码，邮件源密码
    mail.password=xxxx
    #忘记密码，邮件标题
    mail.title=忘记密码
    #忘记密码，邮件内容
    mail.content=亲爱的{0},您好：<br/><br/><br/>您在申请找回密码，重设密码地址：<br/><a href={1} target='_BLANK'>{1}</a> <br/><br/>如果上面的链接无法点击，您也可以复制链接，粘贴到您浏览器的地址栏内，然后按“回车”打开重置密码页面。本邮件超过24小时,链接将会失效，需要重新申请找回密码。<br/><br/>如果您没有进行过找回密码的操作，请不要点击上述链接。<br/><br/><br/><br/>谢谢!
    #忘记密码，返回登录页
    forgetPwd.loginPage=http://127.0.0.1:8080/security/login!toLogin.action
    
    #此处配置原生JDBC链接（非bonc）可选择oracle或者mysql
    #database.type=oracle
    #database.original.driver=oracle.jdbc.driver.OracleDriver
    #database.original.url=jdbc:oracle:thin:@192.168.0.169:1521:orcl
    #database.username=security_hn
    #database.password=security_hn
    
    #for mysql
    #database.type=mysql
    #database.original.driver=com.mysql.jdbc.Driver
    #database.original.url=jdbc:mysql://DB_IP:DB_PORT/DB_NAME
    #database.username=DB_USERNAME
    #database.password=DB_PASSWD
    
    # for license
    license=false
    
    #新需求所需配置文件 (邮箱发送密码将要过期模板；短信发送密码将要过期模板)
    #发送信息方式(email,phone,all)
    sendMessage.type=email 
    #密码将要过期，内容
    passwordExpire.content=【温馨提示：您的账号{0}密码还有{1}天过期，密码过期后将无法登录；请点击链接<a href={2} target='_BLANK'>{2}</a>进行密码修改】
    #用户冻结信息，内容
    userinfoFreeze.content=【温馨提示：您的账号{0},由于{1}实现了冻结，日后将不能正常使用系统，有任何疑问联系管理员】
    #用户解冻信息，内容
    userinfoThaw.content=【温馨提示：您的账号{0},由于{1}实现了解冻，可以正常使用系统了，有任何疑问联系管理员】
    


​    
```
b. WEB-INF/classes/cfg.db.deploy.properties
```


```
...

#此处修改成你用的数据库的信息，驱动，连接url及用户名和密码(bonc的驱动)
#######################datasource config################################
#for oracle
#database.type=oracle
#database.driver=com.bonc.jdbc.OracleDriver
#database.url=jdbc:bonc:oracle:thin:@DB_IP:DB_PORT:DB_NAME
#database.username=DB_USERNAME
#database.password=DB_PASSWD

#for mysql
#database.type=mysql
#database.driver=com.bonc.jdbc.MysqlDriver

#database.url=jdbc:bonc:mysql://DB_IP:DB_PORT/DB_NAME?autoReconnect=true&amp;useUnicode=true&amp;characterEncoding=UTF8&amp;tentantNoIgnore=PERMISSION

#database.username=DB_USERNAME
#database.password=DB_PASSWD

#for h2
#database.type=h2
#database.driver=net.sf.hajdbc.sql.Driver
#database.url=jdbc:bonc:ha-jdbc:cluster1
#database.username=sa
#database.password=


#for db2
#database.type=db2
#database.driver=com.ibm.db2.jcc.DB2Driver
#database.url=jdbc:db2://192.168.6.26:50110/bpm_test
#database.username=bpm_test
#database.password=123456
#database.type=db2
#database.driver=com.ibm.db2.jcc.DB2Driver
#database.url=jdbc:db2://10.240.1.121:60000/udasprd:traceLevel=3;driverType=4;currentSchema=C##BLPT;
#database.username=udas
#database.password=Touchse123$

#for postgresql
#database.driver=org.postgresql.Driver
#database.url=jdbc:postgresql://192.168.6.12:5432/pure
#database.username=pure
#database.password=pure
database.initialPoolSize=3
database.acquireIncrement=3
database.minPoolSize=3
database.maxPoolSize=100
database.maxIdleTime=120
database.encryptType=0

#######################hibernate config################################
#hibernate.dialect=org.hibernate.dialect.HSQLDialect
#hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
#hibernate.dialect=org.hibernate.dialect.Oracle9iDialect
#hibernate.dialect=org.hibernate.dialect.Oracle10gDialect
#此处修改成你用的数据库的方言类理 
hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
#hibernate.dialect=org.hibernate.dialect.MySQLInnoDBDialect 
#hibernate.dialect=org.hibernate.dialect.DB2Dialect
#hibernate.dialect=org.hibernate.dialect.H2Dialect
hibernate.show_sql=false
hibernate.hbm2ddl.auto=update
hibernate.jdbc.batch_size=20
#此处修改成你用的数据库的schema 
hibernate.default_schema=DB_SCHEMA

```


​	




```
 c. 然后打开war包路径WEB-INF/web.xml：
```


    ...
    <!-- 租户过滤器 -->
     <filter>
    	<filter-name>tenantFilter</filter-name>
    	<filter-class>com.bonc.security.web.TenantFilter</filter-class>
    	<init-param>
    		<description> 忽略URL </description>
            <param-name>skipUrls</param-name>
            <param-value>.*/tenantinfo!selectTenant.action.*,.*/tenantinfo!getOrSaveTenantCookie.action.*,.*/tenantinfo!.*,.*/tenantDataShare.*,.*health.*,.*api-docs.*,.*tpl$,.*css$,.*js$,.*jpg$,.*jpeg$,.*bmp$,.*png$,.*gif$,.*ico$</param-value>
        </init-param>
    </filter>
    <filter-mapping>
    	<filter-name>tenantFilter</filter-name>
    	<url-pattern>/*</url-pattern>
    </filter-mapping> 
     <!-- 是否用单点登录 ，以及单点退出地址 ,在门户页面中安全退中用-->
     <context-param>
        <param-name>isCAS</param-name>
        <param-value>true</param-value>
    </context-param>
    <context-param>
        <param-name>casLogout</param-name>
        <param-value>/cas/logout?service=/portal</param-value>
    </context-param>
    <!-- 是否用单点  end-->
    <filter>
    	<description> 单点登录,如果使用单点登出，须配置SingleSignOutHttpSessionListener监听器  </description>
    	<filter-name>SSO Filter</filter-name>
    	<filter-class>com.bonc.sso.client.SSOFilter</filter-class>
    	<init-param>
    		<description> CAS客户端地址 </description>
    		<param-name>serverName</param-name>
    		<param-value>127.0.0.1:8080</param-value>
    	</init-param>
    	<init-param>
    		<description> CAS服务器地址全路径 </description>
    		<param-name>casServerUrlPrefix</param-name>
    		<param-value>http://127.0.0.1:8080/cas</param-value>
    	</init-param>
    	<init-param>
    		<description> CAS服务器登录地址 全路径 </description>
    		<param-name>casServerLoginUrl</param-name>
    		<param-value>http://127.0.0.1:8080/cas/login</param-value>
    	</init-param>
    	<init-param>
    		<description> 是否启用单点登出 </description>
    		<param-name>singleSignOut</param-name>
    		<param-value>true</param-value>
    	</init-param>
    	<init-param>
    		<description> 单点登录忽略校验URL </description>
    		<param-name>skipUrls</param-name>
    		<param-value>.*/tenantinfo!selectTenant.action.*,.*/tenantinfo!getOrSaveTenantCookie.action.*,.*/reg!.*,.*/rest/.*,.*/login!exit.action,.*health.*,.*api-docs.*,/out.jsp,/index.jsp,/struts/.*,/resources/*,.*tpl$,.*css$,.*js$,.*jpg$,.*jpeg$,.*bmp$,.*png$,.*gif$,.*ico$</param-value>
    	</init-param>
    	<init-param>
    		<description> 登录成功后的的用户信息准备 须实现com.bonc.pure.sso.client.ILoginUserHand 接口 </description>
    		<param-name>loginUserHandle</param-name>
    		<param-value>com.bonc.security.sso.SSOAuthHandle</param-value>
    	</init-param>
    	<init-param>
    		<description> 可选参数，客户端应用使用的字符集，如果已经有其他的地方设置过了，则会忽略这个配置。默认将使用UTF-8作为默认字符集 </description>
    		<param-name>characterEncoding</param-name>
    		<param-value>UTF-8</param-value>
    	</init-param>
    	<init-param>
    		<description> 解决读取CAS server端返用户扩展信息中文乱码问题 </description>
    		<param-name>encoding</param-name>
    		<param-value>UTF-8</param-value>
    	</init-param>
    </filter> 
    <filter-mapping>
    	<filter-name>SSO Filter</filter-name>
    	<url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!-- LoginFilter 登录过滤-->
    <!-- <filter>
    	<filter-name>LoginFilter</filter-name>
    	<filter-class>com.bonc.security.web.LoginFilter</filter-class>
    	<init-param>
    		<description> 忽略URL </description>
            <param-name>skipUrls</param-name>
            <param-value>.*/reg!.*,.*/login!.*,.*/pwdinfo!forgetPwd.*,.*/rest/.*,.*health.*,.*api-docs.*,.*image.jsp.*,.*index.jsp.*,.*tpl$,.*css$,.*js$,.*jpg$,.*jpeg$,.*bmp$,.*png$,.*gif$,.*ico$</param-value>
        </init-param>
    	<init-param>
    		<description>登录地址</description>
    		<param-name>loginUrl</param-name>
    		<param-value>/security/login!toLogin.action</param-value>
    	</init-param>
                
    	<init-param>
    		<description>指定登录成功后缺省页面 , 一般指向门户导航页面</description>
    		<param-name>defaultPage</param-name>
    		<param-value>/portal/</param-value>
    	</init-param>
                
    </filter>
    <filter-mapping>
    	<filter-name>LoginFilter</filter-name>
    	<url-pattern>/*</url-pattern>
    </filter-mapping> -->
    
    <!-- 权限过滤器 -->
    <filter>
    	<filter-name>authFilter</filter-name>
    	<filter-class>com.bonc.security.web.AuthFilter</filter-class>
    	<init-param>
    		<description> 忽略URL </description>
            <param-name>skipUrls</param-name>
            <param-value>.*/tenantinfo!selectTenant.action.*,.*/tenantinfo!getOrSaveTenantCookie.action.*,.*/reg!.*,.*/login!.*,.*/pwdinfo!.*,.*health.*,.*api-docs.*,.*tpl$,.*css$,.*js$,.*jpg$,.*jpeg$,.*bmp$,.*png$,.*gif$,.*ico$</param-value>
        </init-param>
    </filter>
    <filter-mapping>
    	<filter-name>authFilter</filter-name>
    	<url-pattern>*.action</url-pattern>
    </filter-mapping>
    <filter-mapping>
    	<filter-name>authFilter</filter-name>
    	<url-pattern>/WEB-INF/pages/*</url-pattern>
    </filter-mapping>
    <filter-mapping>
    	<filter-name>authFilter</filter-name>
    	<url-pattern>/rest/*</url-pattern>
    </filter-mapping>
    <!-- UI过滤器 -->
    <!--  <filter>
    	<filter-name>uiFilter</filter-name>
    	<filter-class>com.bonc.security.web.UIFilter</filter-class>
     <init-param>
            <description> 忽略URL </description>
            <param-name>skipUrls</param-name>
            <param-value>.*/rest/.*,.*health.*,.*api-docs.*,.*\.(css|js|jpg|jpeg|png|gif|ico|woff)$</param-value>
     </init-param>
    </filter>
    <filter-mapping>
    	<filter-name>uiFilter</filter-name>
    	<url-pattern>/*</url-pattern>
    </filter-mapping> -->
        
    <!-- 日志过滤 -->
    <!-- <filter>
    	<filter-name>LogFilter</filter-name>
    	<filter-class>com.bonc.security.web.LogFilter</filter-class>
    	<init-param>
    		<description> 忽略URL </description>
    		<param-name>skipUrls</param-name>
    		<param-value>.*/login!.*,.*health.*,.*api-docs.*</param-value>
    	</init-param>
    </filter>
    
    <filter-mapping>
    	<filter-name>LogFilter</filter-name>
    	<url-pattern>*.action</url-pattern>
    </filter-mapping> -->
    ....




```
d. 修改其他应用war包路径WEB-INF/web.xml
```

将租户过滤器tenantfilter放在SSO filter之前



```
e. 修改 bonc-security-base.properties
```

```
# 指定应用模块的编号, 用于按当前应用编号过滤相关权限数据信息
appCode = security

#指定api调用方式(REST/local)， RESTful或java, 如果是前者， 则必须指定安全服务主地
apiType = local

#指定安全服务的主地址（）
securityServiceURL=http://SE_IP:SE_PORT/security

#记录日志是否在文件中记录(默认关闭)
recordLogInFile=false

#记录日志文件的目录(到文件夹级别,日志文件目录会按照一定规则生成)
log.file=F:/security/logs

#配置是否需要账号冻结与否验证(默认关闭)
accountLockCheck=true 

#配置是否需要账号密码定期修改(默认关闭)
accountPwdCheck=true

#配置账号定期修改时间(单位:day,默认60day)
space.time=60

#配置是否需要账号长期未登陆过期验证(默认关闭)
accountCheck=true

#密码到期7天前提醒
remind.time=7

#配置过期时间(单位：day,默认90天)
limit.time=90

#登录信息存到cookie
login.cookie=true
 
#跨域开关
crossorigin=true
 
#开启选租户true|false
chooseTenant=true

```




```
f. 修改其他应用的pom.xml
```

将bonc-security-base的依赖版本改为1.2.2-SNAPSHOT（参考）

		<dependency>
			<groupId>com.bonc</groupId>
			<artifactId>bonc-security-base</artifactId>
			<version>1.2.2-SNAPSHOT</version>
			<exclusions>
				<exclusion>
					<groupId>com.oracle</groupId>
					<artifactId>ojdbc14</artifactId>
				</exclusion>
			</exclusions>
		</dependency>


```
g. 是否启用多租户
```

启用多租户：
​		
WEB-INF/classes/bonc-jdbc.properties	下	

```
sqltranforms=com.bonc.security.transform.TenantSQLTransform,com.bonc.security.transform.PrivilegeSQLTransform
```

不启用多租户：	

打开各个war包下路径WEB-INF/web.xml 注释掉租户过滤器tenantFilter

WEB-INF/classes/bonc-jdbc.properties	下	

```
sqltranforms=com.bonc.security.transform.PrivilegeSQLTransform
```

  

2.部署war包
​	 
部署war包到web服务器。  如，将war放在tomcat主目录下/webapps/下。


3.启动web服务器

 访问  http://localhost:8080/security,  如果出现选择租户页面就说明基本部署成功。


## 部署补充
### mysql数据库配置字符集
首先查看 数据库字符集 show variables like 'character_set_%';
​     
### tomcat启动慢
（1）在catalina.sh中加入这么一行：-Djava.security.egd=file:/dev/./urandom
（2）打开$JAVA_PATH/jre/lib/security/java.security这个文件，找到下面的内容：
	securerandom.source=file:/dev/urandom 替换成 securerandom.source=file:/dev/./urandom

### manager/WEB-INF/web.xml配置max-file-size大于100m

### 调整catalina.sh参数 
JAVA_OPTS="-Xms1g -Xmx2g  -XX:PermSize=256M -XX:MaxPermSize=512M "

###配置tomcat-users.xml 末尾添加
	<role rolename="tomcat"/>
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<user username="manager" password="manager" roles="manager-gui,manager-script"/>

### 要使用专用tomcat,否则400错误
