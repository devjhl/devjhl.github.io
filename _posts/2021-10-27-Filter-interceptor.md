---
title:  "Spring Interceptor vs Servlet Filter"
excerpt: "InterceptorとFilter整理する"

categories:
  - Spring
  - Servlet
tags:
  - Interceptor
  - Filter

toc: true
toc_sticky: true
---

Spring mvc講座
<https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2>


#### サーブレットフィルタ
- アプリケーション色々なロジックで共通に関心あるものを、共通の関心事（cross-cutting　concern）という。

**フィルタ流れ**<br>
HTTPリクエスト→WAS→フィルタ→サーブレット->コントローラー

**フィルタチェイン**<br>
HTTPリクエスト→WAS→フィルタ1→フィルタ2→フィルタ3→サーブレット->コントローラー

```java
public interface Filter {

  public default void init(FilterConfig filterConfig) throws ServletException{}

  public void doFilter(ServletRequest request, ServletResponse response,
          FilterChain chain) throws IOException, ServletException;
  public default void destroy() {} 

}
```

**サーブレットフィルタ-認証チェック**
```java
@Slf4j
public class LoginCheckFilter implements Filter {

   private static final String[] whitelist = {"/", "/members/add", "/login","/logout","/css/*"};
 
  @Override
  public void doFilter(ServletRequest request, ServletResponse response,FilterChain chain) 
      throws IOException, ServletException {
      
      HttpServletRequest httpRequest = (HttpServletRequest) request;
      String requestURL = httpRequest.getRequestURI();

      HttpServletResponse httpResponse = (HttpServletResponse) response;
 
      try {
        log.info("認証チェックスタート{}", requestURI);

        if(isLoginCheckPath(requestURI)) {
              log.info("認証チェックロジック実行{}", requestURI);
              HttpSession session = httpRequest.getSession(false);
              if(session == null || session.getAttribute(SessionConst.LOGIN_MEMBER) == null ) {
                log.info("未認証使用者リクエスト{}", requestURI);
                //loginにリダイレクト
                httpResponse.sendRedirect("/login?redirectURL=" + requestURI);

                return;
              }
        }
        chain.doFilter(request, response);
     } catch (Exception e) {
       throw e; 
     } finally {
       log.info("認証チェックフィルタ終了 {}", requestURI);
     }
      }
  // whitelist場合は認証チェックしない
  private boolean isLoginCheckPath(String requestURI) {
    return !PatternMatchUtils.simpleMatch(whitelist, requestURI);
  }
}

@Bean
public FilterRegistrationBean loginCheckFilter() {
   FilterRegistrationBean<Filter> filterRegistrationBean = new
FilterRegistrationBean<>();
   filterRegistrationBean.setFilter(new LoginCheckFilter());
  filterRegistrationBean.setOrder(2);
   filterRegistrationBean.addUrlPatterns("/*");
   return filterRegistrationBean;
}
```
      
```java
@PostMapping("/login")
public String loginV4(
 @Valid @ModelAttribute LoginForm form, BindingResult bindingResult,
 @RequestParam(defaultValue = "/") String redirectURL,
 HttpServletRequest request) {
 if (bindingResult.hasErrors()) {
 return "login/loginForm";
 }
 Member loginMember = loginService.login(form.getLoginId(),
form.getPassword());
 log.info("login? {}", loginMember);
 
 if (loginMember == null) {
    bindingResult.reject("loginFail", "IDとパスワードが違います。");
     return "login/loginForm";
 }
 HttpSession session = request.getSession();

 session.setAttribute(SessionConst.LOGIN_MEMBER, loginMember);

 return "redirect:" + redirectURL;
}

```

#### インターセプター

**インターセプター流れ**
- HTTPリクエスト→WAS→フィルタ→サーブレット→Springインターセプター→コントローラー

**インターセプターチェイン**
- HTTPリクエスト→WAS→フィルター→サーブレット→インターセプター１→インターセプター２→コントローラー

```java
public interface HandlerInterceptor {
  default boolean preHandle(HttpServletRequest request, HttpServletResponse 
response,Object handler) throws Exception {}
  default void postHandle(HttpServletRequest request, HttpServletResponse 
response,Object handler, @Nullable ModelAndView modelAndView) throws Exception {}
  default void afterCompletion(HttpServletRequest request, HttpServletResponse 
response,Object handler, @Nullable Exception ex) throws Exception {}
}
```
**インターセプター認証チェック**
```java
@Slf4j
public class LoginCheckInterceptor implements HandlerInterceptor {

  @Overrid
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

    String requestURI = request.getRequestURI();

    log.info("認証チェックインターセプター実行{}", requestURI);
    HttpSession session = request.getSession(false);

    if(session == null || session.getAttribute(SessionConst.LOGIN_MEMBER) == null) {
      log.info("未認証使用者リクエスト");
      //ログインでリダイレクト
      response.sendRedirect("/login?redirectURL" + requestURI);
      return false;
    }
    return true;
  }

}  

@Configuration
public class WebConfig implements WebMvcConfigurer {

  @Override
  public void addInterceptors(InterceptorRegistry registry) {

    registry.addInterceptor(new LoginCheckInterceptor()) {
            .order(2)
            .addPathPatterns("/**")
            .excluedPathPatterns(
               "/", "/members/add", "/login", "/logout",
              "/css/**", "/*.ico", "/error" 
            );
    }

  }
}




```


---