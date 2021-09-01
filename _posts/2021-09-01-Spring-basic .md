---
title:  "Spring基礎"
excerpt: "Spring基礎整理しておく"

categories:
  - Spring
tags:
  - 基礎

toc: true
toc_sticky: true
---

## DI(依存性の注入)
- オブジェクトの利用者自身ではくDIコンテナがオブジェクトの生成と注入を行う

## DIコンテナ
- オブジェクトの生成して管理しながら、依存関係を連結するこという

アノテーションによるDIの設定
クラス名上に<code>@Configuration</code>
## Springコンテナ

 **直接注入**

<code>@Configuration</code>
1.  Springコンテナ生成
2.  構成情報活用
<code><@Bean></code>はメソッド名を作用
> 名前を定義することもできす。でも、いつ他の名前で付与しなればならない

<code>@Bean(name="memberSservice")</code>

## Bean照会　ー　基本
```java
 @Test
    @DisplayName("BeanName")
    void findBeanByName() {
        MemberService memberService = ac.getBean("memberService", MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }

    @Test
    @DisplayName("BeanType")
    void findBeanByType() {
        MemberService memberService = ac.getBean(MemberService.class);
        assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
    }
```

## Java code,XML
- コンパイルなしにBean設定情報を変更できできる

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans http://
www.springframework.org/schema/beans/spring-beans.xsd">

 <bean id="memberService" class="hello.core.member.MemberServiceImpl">
	 <constructor-arg name="memberRepository" ref="memberRepository" />
 </bean>

 <bean id="memberRepository"class="hello.core.member.MemoryMemberRepository" />

 <bean id="orderService" class="hello.core.order.OrderServiceImpl">
	 <constructor-arg name="memberRepository" ref="memberRepository" />
	 <constructor-arg name="discountPolicy" ref="discountPolicy" />
 </bean>
 
<bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy" />

</beans>
```
```java
public class XmlAppContext {
	 @Test
	 void xmlAppContext() {
		 ApplicationContext ac = new GenericXmlApplicationContext("appConfig.xml");
	 
	   MemberService memberService = ac.getBean("memberService", MemberService.class);
	   assertThat(memberService).isInstanceOf(MemberService.class);
	 }
}
```
**ComponentScanと自動注入**
- SpringBeanの基本名前はクラス名の一番目を小文字
- 名前
  - @Component("memberService")  



## シングルトン
- オブジェクトインスタンスをシングルトン(1個だけ生成)で管理する
- Springコンテナがシングルトンコンテナ役割をする

！地域変数、パラメータ、ThreadLocalなど使使用

<code>@Autowired</code>
注入コンストラクタに@Autowiredをすると、Springコンテナが自動で該当SpringBeanを探して注入する  
getBean...と同一すると考えよう  
> 注入する対象がないとエラーが発生
　注入する対象がなくても動作するとしたら<code>Autowired(required = false)</code>

**おすすめ**
設定情報クラスの位置をプロジェクト一番上に置く  
ex)
- com.hello -> ここに<code>@ConponentScan</code>
- com.hello.service
- com.hello.repository

コンポーネントスキャン基本対象
> @Controller  
  @Repository : Springデータ認識
　@Configuration : Spring設定情報認識
  @Service : ビジネスロジック
  
  **setter注入(選択、変更)**  

  ## コンストラクタ注入(推奨)
  - **不変、必須**
  - **コンストラクタが一つだけあれば<code>@Autowired</code>**省略しても自動で注入される(Spring Bean) 

  ```java
@Component
public class OrderServiceImpl implements OrderService {

 private final MemberRepository memberRepository;
 private final DiscountPolicy discountPolicy;

 public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy 
discountPolicy) {

 this.memberRepository = memberRepository;

 this.discountPolicy = discountPolicy;
	 }
}
```

## オプション

```java
public class AutowiredTest {

   @Test
    void AutowiredOption() {
       ApplicationContext ac = new AnnotationConfigApplicationContext(TestBean.class);
   }

   static class TestBean {
     // 呼び出しなし	
       @Autowired(required = false)
       public void setNoBean1(Member noBean1) {
           System.out.println("noBean1" + noBean1);
       }
      //null 呼び出し
       @Autowired
       public void setNoBean2(@Nullable Member noBean2) {
           System.out.println("noBean2" + noBean2);
       }
       //Optionnal.empty 呼び出し
       @Autowired
       public void setNoBean3(Optional<Member> noBean3) {
           System.out.println("noBean3" + noBean3);
       }
   }
}
```
**照会Beanが2つ以上時？**
> @Qualifier

```java
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy {}

@Component
@Qualifier("fixDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy {}
```

```java
@Autowired
public OrderServiceImpl(MemberRepository memberRepository,
 @Qualifier("mainDiscountPolicy") DiscountPolicy 
discountPolicy) {
 this.memberRepository = memberRepository;
 this.discountPolicy = discountPolicy;
}
```

**優先順位**
>@Primary

```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy {}
@Component
public class FixDiscountPolicy implements DiscountPolicy {}
}
```
**照会したBeanが全部必要な時(list、Map)**

```java
public class AllBeanTest {

   @Test
    void findAllBean() {
       ApplicationContext ac = new AnnotationConfigApplicationContext(AutoAppConfig.class,DiscountService.class);
       DiscountService discountService = ac.getBean(DiscountService.class);
      
　　　 Member member = new Member(1L,"userA", Grade.VIP);
       int discountPrice = discountService.discount(member, 10000, "fixDiscountPolicy");

       assertThat(discountService).isInstanceOf(DiscountService.class);
       assertThat(discountPrice).isEqualTo(1000);

   }

   static class DiscountService {
       private final Map<String, DiscountPolicy> policyMap;
   
       private final List<DiscountPolicy> policies;

       public DiscountService(Map<String, DiscountPolicy> policyMap, List<DiscountPolicy> policies) {
           this.policyMap = policyMap;
           this.policies = policies;
           System.out.println("policyMap = " + policyMap);
           System.out.println("policies = " + policies);
       }

       public int discount(Member member, int price , String discountCode) {
        
           DiscountPolicy discountPolicy = policyMap.get(discountCode);

           System.out.println("discountCode = " + discountCode);
           System.out.println("discountPolicy = " + discountPolicy);

           return discountPolicy.discount(member, price);
       }
   }
```

## アノテーション @PostConstruct  @PreDestory
- 初期化： @PostConstructをメソッド上に宣言
- 終了 :  @PreDestoryをメソッド上に宣言

```java
@PostConstruct
public void init() throws Exception {
    connect();
    call("初期化メッセージ");
}
@PreDestory
public void init() throws Exception {
    disconnect();
}
```

SpringコンテナはprototypeBeanを生成して、依存関係注入、初期化までだけ処理してくれる
それで<code>@ProDestory</code>見たいな終了メソッドは呼び出さない

## ウェブスコープ
- request ：要請一つ入って出るまでは維持するスコープそれぞれのHTTP要請ごとに別の空くインスタンスが生成されて、管理される
- session : HTTP Session同一
- application : ServletContext
- websocket 



---
出所 :　https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1#
---