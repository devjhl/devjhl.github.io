---
title:  "Servlet Exception - エラーページ"
excerpt: "エラーページを提供する"

categories:
  - Servlet
tags:
  - Exception

toc: true
toc_sticky: true
---

Spring mvc講座
<https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2>

```java
@Component
public class WebServerCustomizer implements
        WebServerFactoryCustomizer<ConfigurableWebServerFactory> {
    @Override
    public void customize(ConfigurableWebServerFactory factory) {

        ErrorPage errorPage404 = new ErrorPage(HttpStatus.NOT_FOUND, "/error-page/400");
        ErrorPage errorPage500 = new ErrorPage(HttpStatus.INTERNAL_SERVER_ERROR, "/error-page/500");
        ErrorPage errorPageEx = new ErrorPage(RuntimeException.class, "/error-page/500");

        factory.addErrorPages(errorPage404,errorPage500,errorPageEx);
    }
}

@Slf4j
@Controller
public class ErrorPageController {
 
  @RequestMapping("/error-page/404")
  public String errorPage404(HttpServletRequest request, HttpServletResponse 
response) {
     log.info("errorPage 404");
     return "error-page/404";
  }
  @RequestMapping("/error-page/500")
  public String errorPage500(HttpServletRequest request, HttpServletResponse 
response) {
     log.info("errorPage 500");
     return "error-page/500";
  }
}

```

1. Exceptionが発生してWASまで行く
2. WASはエラーページ径路を探して内部でエラーページを呼び出す。この時エラーページ径路でフィルター、
サーブレット、インターセプター、コントローラーが全部もう一度呼び出す。　　

WASはエラー情報をrequestのattributeに追加して引渡す<br>
必要ならエラーページで伝達したエラー情報を使用できる　　

```java
@Slf4j
@Controller
public class ErrorPageController {

 public static final String ERROR_EXCEPTION =
"javax.servlet.error.exception";
 public static final String ERROR_EXCEPTION_TYPE =
"javax.servlet.error.exception_type";
 public static final String ERROR_MESSAGE = "javax.servlet.error.message";
 public static final String ERROR_REQUEST_URI =
"javax.servlet.error.request_uri";
 public static final String ERROR_SERVLET_NAME =
"javax.servlet.error.servlet_name";
 public static final String ERROR_STATUS_CODE =
"javax.servlet.error.status_code";

 @RequestMapping("/error-page/404")
 public String errorPage404(HttpServletRequest request, HttpServletResponse 
response) {
 log.info("errorPage 404");
 printErrorInfo(request);
 return "error-page/404";
 }

 @RequestMapping("/error-page/500")
 public String errorPage500(HttpServletRequest request, HttpServletResponse 
response) {
 log.info("errorPage 500");
 printErrorInfo(request);
 return "error-page/500";
 }

 private void printErrorInfo(HttpServletRequest request) {
 log.info("ERROR_EXCEPTION: ex=",
request.getAttribute(ERROR_EXCEPTION));
 log.info("ERROR_EXCEPTION_TYPE: {}",
request.getAttribute(ERROR_EXCEPTION_TYPE));
 log.info("ERROR_MESSAGE: {}", request.getAttribute(ERROR_MESSAGE)); 
 log.info("ERROR_REQUEST_URI: {}",
request.getAttribute(ERROR_REQUEST_URI));
 log.info("ERROR_SERVLET_NAME: {}",
request.getAttribute(ERROR_SERVLET_NAME));
 log.info("ERROR_STATUS_CODE: {}",
request.getAttribute(ERROR_STATUS_CODE));
 log.info("dispatchType={}", request.getDispatcherType());
 }
}
```

**DispatcherType**
- クライアントから発生した正常リクエストか、もしくはエラーページを出力するための内部リクエストか区分なのか区分できなければならない。サーブレットは<code>'DispatcherType'</code>という追加情報を提供する

```java
  @Slf4j
  public class LogFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    log.info("log filter init");
  }

  @Override
  public void doFilter(ServletRequest request, ServletResponse response,
    FilterChain chain) throws IOException, ServletException {
      HttpServletRequest httpRequest = (HttpServletRequest) request;
      String requestURI = httpRequest.getRequestURI();
      String uuid = UUID.randomUUID().toString();
     
      try {
         log.info("REQUEST [{}][{}][{}]", uuid,
         request.getDispatcherType(), requestURI);
         chain.doFilter(request, response);
      } catch (Exception e) {
         throw e;
      } finally {
        log.info("RESPONSE [{}][{}][{}]", uuid,
        request.getDispatcherType(), requestURI);
        }
      }
    @Override
    public void destroy() {
      log.info("log filter destroy");
      }
    }

    @Configuration
    public class WebConfig implements WebMvcConfigurer {

    @Bean
    public FilterRegistrationBean logFilter() {
      FilterRegistrationBean<Filter> filterRegistrationBean = newFilterRegistrationBean<>();
      filterRegistrationBean.setFilter(new LogFilter());
      filterRegistrationBean.setOrder(1);
      filterRegistrationBean.addUrlPatterns("/*");
      filterRegistrationBean.setDispatcherTypes(DispatcherType.REQUEST,DispatcherType.ERROR);
      return filterRegistrationBean;
      }
    }
```

---