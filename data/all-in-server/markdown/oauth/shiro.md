### 发一份shiro标准配置，特此记录

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:util="http://www.springframework.org/schema/util"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">

    <description>Shiro安全配置</description>

    <!--安全管理器-->
    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <!--设置自定义Realm-->
        <property name="realm" ref="shiroDbRealm"/>
        <!--将缓存管理器，交给安全管理器-->
        <property name="cacheManager" ref="shiroEhcacheManager"/>
        <!-- 注入session管理器 -->
        <property name="sessionManager" ref="sessionManager" />
        <!-- 记住密码管理 -->
<!--         <property name="rememberMeManager" ref="rememberMeManager"/> -->
    </bean>

    <!-- 项目自定义的Realm -->
    <bean id="shiroDbRealm" class="com.agood.bejavagod.shiro.ShiroDbRealm"/>

    <!-- 记住密码Cookie -->
    <bean id="rememberMeCookie" class="org.apache.shiro.web.servlet.SimpleCookie">  
        <constructor-arg value="rememberMe"/>
        <property name="httpOnly" value="true"/>
        <!-- 7天,采用spring el计算方便修改[细节决定成败]！ -->
        <property name="maxAge" value="#{7 * 24 * 60 * 60}"/>
<!--         <property name="domain" value=".bejavagod.com"/> -->
    </bean>

    <!-- rememberMe管理器,cipherKey生成见{@code Base64Test.java} -->
    <bean id="rememberMeManager" class="org.apache.shiro.web.mgt.CookieRememberMeManager">
        <property name="cipherKey" value="#{T(org.apache.shiro.codec.Base64).decode('5aaC5qKm5oqA5pyvAAAAAA==')}"/>
        <property name="cookie" ref="rememberMeCookie"/>  
    </bean>

    <!-- Shiro Filter -->
    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <!-- 安全管理器 -->
        <property name="securityManager" ref="securityManager"/>
        <!-- 默认的登陆访问url -->
        <property name="loginUrl" value="/login.action"/>
        <!-- 登陆成功后跳转的url -->
        <property name="successUrl" value="/index.action"/>
        <!-- 没有权限跳转的url -->
        <property name="unauthorizedUrl" value="/unauth.action"/>
        
<!--         自定义filter配置 -->
        <property name="filters">
            <map>
                <entry key="authc">
                    <bean class="com.agood.bejavagod.controller.filter.CustomFormAuthenticationFilter"></bean>
                </entry>
            </map>
        </property>
        
        <property name="filterChainDefinitions">
            <value>
                <!-- 
                    anon  不需要认证
                    authc 需要认证
                    user  验证通过或RememberMe登录的都可以
                -->
<!--                 /commons/** = anon -->
                /static/** = anon
<!--                 /webhooks = anon -->
                /login.action = anon
                
                /page/404.action = anon
                /page/500.action = anon
                
<!--                 /dataDict/saveOrUpdateDataDict.action = perms["shiro:save"] -->
                
                /** = authc
            </value>
        </property>
    </bean>

    <!-- 用户授权信息Cache, 采用EhCache -->
    <bean id="shiroEhcacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
        <property name="cacheManagerConfigFile" value="classpath:shiro/ehcache-shiro.xml"/>
    </bean>

    <!-- 在方法中 注入  securityManager ，进行代理控制 -->
    <bean class="org.springframework.beans.factory.config.MethodInvokingFactoryBean">
        <property name="staticMethod" value="org.apache.shiro.SecurityUtils.setSecurityManager"/>
        <property name="arguments" ref="securityManager"/>
    </bean>

    <!-- 保证实现了Shiro内部lifecycle函数的bean执行 -->
    <bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>

    <!-- AOP式方法级权限检查  -->
    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor"/>

    <!-- 启用shrio授权注解拦截方式 -->
    <bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
        <property name="securityManager" ref="securityManager"/>
    </bean>
    
    <!-- 会话管理器 -->
<!--     <bean id="sessionManager" class="org.apache.shiro.web.session.mgt.DefaultWebSessionManager"> -->
        <!-- session的失效时长，单位毫秒 1小时: 3600000 -->
<!--         <property name="globalSessionTimeout" value="3600000"/> -->
        <!-- 删除失效的session -->
<!--         <property name="deleteInvalidSessions" value="true"/> -->
<!--     </bean> -->
    
    
    <!-- 会话管理器 start -->
    <bean id="sessionManager" class="org.apache.shiro.web.session.mgt.DefaultWebSessionManager">
        <!-- session的失效时长，单位毫秒 1小时: 3600000 -->
        <!-- 设置全局会话超时时间，默认30分钟，即如果30分钟内没有访问会话将过期 1800000 -->
        <property name="globalSessionTimeout" value="1800000"/>
        <!-- 删除失效的session -->
        <property name="deleteInvalidSessions" value="true"/>
        <!-- 是否开启会话验证器，默认是开启的 -->
        <property name="sessionValidationSchedulerEnabled" value="true"/>
        <!-- 
            Shiro提供了会话验证调度器，用于定期的验证会话是否已过期，如果过期将停止会话；
            出于性能考虑，一般情况下都是获取会话时来验证会话是否过期并停止会话的；
            但是如在web环境中，如果用户不主动退出是不知道会话是否过期的，因此需要定期的检测会话是否过期，
            Shiro提供了会话验证调度器SessionValidationScheduler来做这件事情。
         -->
        <property name="sessionValidationScheduler" ref="sessionValidationScheduler"/> 
        <!-- Shiro提供SessionDAO用于会话的CRUD -->
        <property name="sessionDAO" ref="sessionDAO"/>
        <!-- 
            是否启用/禁用Session Id Cookie，默认是启用的；
            如果禁用后将不会设置Session Id Cookie，即默认使用了Servlet容器的JSESSIONID，
            且通过URL重写（URL中的“;JSESSIONID=id”部分）保存Session Id。 
        -->
        <property name="sessionIdCookieEnabled" value="true"/>
        <property name="sessionIdCookie" ref="sessionIdCookie"/>
    </bean>
    <!-- 会话验证调度器 -->
    <bean id="sessionValidationScheduler" class="org.apache.shiro.session.mgt.ExecutorServiceSessionValidationScheduler">
        <!-- 设置调度时间间隔，单位毫秒，默认就是1小时 -->
        <property name="interval" value="1800000"/>
        <!-- 设置会话验证调度器进行会话验证时的会话管理器 -->
        <property name="sessionManager" ref="sessionManager"/>
    </bean>
<!--     <bean id="sessionValidationScheduler" class="org.apache.shiro.session.mgt.quartz.QuartzSessionValidationScheduler"> -->
<!--         <property name="sessionValidationInterval" value="1800000"/> -->
<!--         <property name="sessionManager" ref="sessionManager"/> -->
<!--     </bean> -->
    <!-- 会话DAO -->
    <bean id="sessionDAO" class="org.apache.shiro.session.mgt.eis.EnterpriseCacheSessionDAO">
        <!-- 设置Session缓存名字，默认就是shiro-activeSessionCache，要和ehcache.xml中的那么对应 -->
        <property name="activeSessionsCacheName" value="shiro-activeSessionCache"/>
<!--         <property name="activeSessionsCacheName" value="shiroCache"/> -->
        <property name="sessionIdGenerator" ref="sessionIdGenerator"/>
    </bean>
    <!-- 会话ID生成器，用于生成会话ID，默认就是JavaUuidSessionIdGenerator，使用java.util.UUID生成-->
    <bean id="sessionIdGenerator" class="org.apache.shiro.session.mgt.eis.JavaUuidSessionIdGenerator"/>
     <!-- 会话Cookie模板，sessionManager创建会话Cookie的模板 -->
    <bean id="sessionIdCookie" class="org.apache.shiro.web.servlet.SimpleCookie">
        <!-- 设置Cookie名字，默认为JSESSIONID -->
<!--         <constructor-arg value="bjg_sid"/> -->
        <!-- 不修改使用默认的话，那么404的时候session就会过期 -->
        <property name="name" value="bjg_sid"/>
        <!-- 
            如果设置为true，则客户端不会暴露给客户端脚本代码，使用HttpOnly cookie有助于减少某些类型的跨站点脚本攻击；
            此特性需要实现了Servlet 2.5 MR6及以上版本的规范的Servlet容器支持
         -->
        <property name="httpOnly" value="true"/>
        <!-- 设置Cookie的过期时间，秒为单位，默认-1表示关闭浏览器时过期Cookie -->
        <property name="maxAge" value="-1"/>
        <!-- 设置Cookie的域名，默认空，即当前访问的域名 -->
<!--         <property name="domain" value=".bejavagod.com"/> -->
    </bean>
    <!-- 会话管理器 end -->

    
    <!-- 自定义form认证过虑器 -->
    <!-- 基于Form表单的身份验证过滤器，不配置将也会注册此过虑器，表单中的用户账号、密码及loginurl将采用默认值，建议配置 -->
<!--         <bean id="formAuthenticationFilter" class="com.agood.bejavagod.controller.filter.CustomFormAuthenticationFilter"> -->
            <!-- 表单中账号的input名称 -->
<!--             <property name="usernameParam" value="username" /> -->
            <!-- 表单中密码的input名称 -->
<!--             <property name="passwordParam" value="password" /> -->
            <!-- 记住我input的名称 -->
<!--             <property name="rememberMeParam" value="rememberMe"/> -->
<!--      </bean> -->
     
</beans>
```