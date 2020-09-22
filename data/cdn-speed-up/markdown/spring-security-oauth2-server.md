# spring-security实现的OAUTH2.0认证服务

### 如果只用spring-security实现OAUTH2.0的认证服务，而不用spring-security做按钮权限，数据权限，接口权限的校验，是不是只需要配置两个类，其中一个类实现AuthorizationServerConfigurerAdapter和一个类实现WebSecurityConfigurerAdapter就可以了?

#### AuthServerConfig.java
```java
@Configuration
@EnableAuthorizationServer
public class AuthServerConfig extends AuthorizationServerConfigurerAdapter {
    @Autowired
    private AuthenticationManager authenticationManager;

    @Override
    public void configure(AuthorizationServerSecurityConfigurer oauthServer) throws Exception {
        oauthServer.tokenKeyAccess("permitAll()")
          .checkTokenAccess("isAuthenticated()");
    }

    @Override
    public void configure(ClientDetailsServiceConfigurer clients) throws Exception {
        clients.inMemory()
          .withClient("SampleClientId")
          .secret("secret")
          .authorizedGrantTypes("authorization_code")
          .scopes("user_info")
          .autoApprove(true);
    }

    @Override
    public void configure(AuthorizationServerEndpointsConfigurer endpoints) throws Exception {
        endpoints.authenticationManager(authenticationManager);
    }
}
```

### 安全配置
#### SecurityConfig.java
```java
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    private AuthenticationManager authenticationManager;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.requestMatchers()
          .antMatchers("/login", "/oauth/authorize")
          .and()
          .authorizeRequests()
          .anyRequest().authenticated()
          .and()
          .formLogin().permitAll();
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.parentAuthenticationManager(authenticationManager)
          .inMemoryAuthentication()
          .withUser("john").password("123").roles("USER");
    }
}
```