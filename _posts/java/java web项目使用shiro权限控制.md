title: java web项目使用shiro权限控制
id: 1442
categories:
  - Shiro
date: 2014-08-09 00:07:23
tags:
---

# 在java web项目中使用shiro控制权限

## pom.xml添加maven依赖

    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-core</artifactId>
        <version>${shiro.version}</version>
    </dependency>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-web</artifactId>
        <version>${shiro.version}</version>
    </dependency>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-ehcache</artifactId>
        <version>${shiro.version}</version>
    </dependency>
    <dependency>
        <groupId>org.apache.shiro</groupId>
        <artifactId>shiro-spring</artifactId>
        <version>${shiro.version}</version>
    </dependency>
    

    ## web.xml中配置shiro的filter

    <filter>
        <filter-name>shiroFilter</filter-name>
        <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
        <init-param>
            <param-name>targetFilterLifecycle</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>

    <filter-mapping>
        <filter-name>shiroFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    

    ## spring bean的配置

    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <property name="securityManager" ref="securityManager" />
        <property name="loginUrl" value="/toLogin" />
        <property name="successUrl" value="/index" />
        <property name="unauthorizedUrl" value="/unauthorized" />
        <!-- The 'filters' property is not necessary since any declared javax.servlet.Filter 
            bean defined will be automatically acquired and available via its beanName 
            in chain definitions, but you can perform overrides or parent/child consolidated 
            configuration here if you like: -->
        <property name="filters">
            <util:map>
                <entry key="perms" value-ref="URLPermissionsFilter" />
            </util:map>
        </property>
        <property name="filterChainDefinitions">
            <value>
                /*.ico = anon
                /*.swf = anon
                /images/*.png = anon
                /images/*.jpg = anon
                /images/*.gif = anon
                /styles/*.css = anon
                /scripts/*.js = anon
                /jplayer/** = anon
                /My97DatePicker/** = anon
                /jstree/** = anon
                /help/** = anon
                /unauthorized = anon
                /toLogin = anon
                /login = anon
                /logout = anon
                /welcome = authc
                /index = authc
                /sound/mobileUpload = anon
                /user/resetPwd = anon
                /404 = anon
                /** = authc,perms
            </value>
        </property>
    </bean>

    <!-- 自定义鉴权拦截器 -->
    <bean id="URLPermissionsFilter" class="com.xxx.filter.URLPermissionsFilter" />
    <bean id="customCredentialsMatcher" class="com.xxx.shiro.CustomCredentialsMatcher"></bean>
    <bean id="customRealm" class="com.xxx.shiro.CustomRealm">
        <property name="credentialsMatcher" ref="customCredentialsMatcher"></property>
    </bean>

    <bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
        <property name="cacheManager" ref="cacheManager" />
        <property name="sessionMode" value="native" />
        <property name="realm" ref="customRealm" />
    </bean>

    <bean id="cacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
        <!-- Set a net.sf.ehcache.CacheManager instance here if you already have one.  If not, a new one
             will be creaed with a default config:
             <property name="cacheManager" ref="ehCacheManager"/> -->
        <!-- If you don't have a pre-built net.sf.ehcache.CacheManager instance to inject, but you want
             a specific Ehcache configuration to be used, specify that here.  If you don't, a default
             will be used.:
        <property name="cacheManagerConfigFile" value="classpath:some/path/to/ehcache.xml"/> -->
    </bean>
    

    ## 代码

    ### 用户访问某url时权限字符串的生成

    public class URLPermissionsFilter extends PermissionsAuthorizationFilter {

        public boolean isAccessAllowed(ServletRequest request, ServletResponse response, Object mappedValue)
                throws IOException {
            return super.isAccessAllowed(request, response, buildPermissions(request));
        }

        /**
         * 根据请求URL产生权限字符串，这里只产生，而比对的事交给Realm
         * 
         * @param request
         * @return
         */
        protected String[] buildPermissions(ServletRequest request) {
            String[] perms = new String[1];
            HttpServletRequest req = (HttpServletRequest) request;
            String path = req.getServletPath();
            path = path.replaceFirst("/", "");
            perms[0] = path;// path直接作为权限字符串
            return perms;
        }
    }
    

    ### 获取用户拥有的权限角色信息AuthorizationInfo和用户名密码AuthenticationInfo

    public class CustomRealm extends AuthorizingRealm {

        @Override
        protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
            // 用户名
            String username = (String) principals.fromRealm(getName()).iterator().next();
            SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
            info.addRoles(角色字符串集合);
            info.addStringPermissions(权限字符串集合);
            return info;
        }

        @Override
        protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authcToken) throws AuthenticationException {
            // 令牌——基于用户名和密码的令牌
            UsernamePasswordToken token = (UsernamePasswordToken) authcToken;
            // 令牌中可以取出用户名
            String username = token.getUsername();
            // 根据用户名去获取密码
            String password = "";
            return new SimpleAuthenticationInfo(username, password, getName());
        }

    }
    

    ### 用户身份验证(验证密码)

    public class CustomCredentialsMatcher extends SimpleCredentialsMatcher {

        @Override
        public boolean doCredentialsMatch(AuthenticationToken token, AuthenticationInfo info) {
             PasswordService svc = new DefaultPasswordService();
             UsernamePasswordToken upToken = (UsernamePasswordToken) token;
             return svc.passwordsMatch(upToken.getCredentials(), info.getCredentials().toString());
        }

    }
    

    ### 登录时验证身份

    UsernamePasswordToken token = new UsernamePasswordToken(userName, password);
    try {
        SecurityUtils.getSubject().login(token);
        // 登陆成功
    } catch (AuthenticationException e) {
        // 登陆失败,用户名或密码错误
    }
    

    ### 是否拥有某权限判断

    if (SecurityUtils.getSubject().hasRole("superadmin")) {
        // yes
    } else {
        // no
    }
    
