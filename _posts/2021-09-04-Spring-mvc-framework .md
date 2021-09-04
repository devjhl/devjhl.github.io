---
title:  "Spring MVCフレームワーク"
excerpt: "Spring MVCフレームワークを整理しておく"

categories:
  - Spring
tags:
  - フレームワーク

toc: true
toc_sticky: true
---

## アノテーション

<code>@RequestMapping</code><br>
**リクエスト情報をマッピングする。<br>
該当URLが呼び出されたらこのメソッドが呼び出される**

<code>@Controller</code><br>
**Springが自動的にSpring Beanを登録する。<br>
(内部に@Componentがある)**

## Spring MVC

```java
**
 * v3
 * Model 導入
 * ViewName 直接返す
 * @RequestParam 使用
 * @RequestMapping -> @GetMapping, @PostMapping
 */
@Controller
@RequestMapping("/springmvc/v3/members")
public class SpringMemberControllerV3 { 

private MemberRepository memberRepository = MemberRepository.getInstance();

 @GetMapping("/new-form")
public String newForm() {
        return "new-form";
}

@PostMapping("/save")
public String save(
		     @RequestParam("username") String username,
         @RequestParam("age") int age,
         Model model) {

        Member member = new Member(username, age);
        memberRepository.save(member);

        model.addAttribute("member",member);
        return "save-result";
    }


 @GetMapping
 public String members(Model model) {
     List<Member> members = memberRepository.findAll();

      model.addAttribute("members",members);
      return "members";

    }
}
```

**RequestParam?**<br>
SpringはHTTPリクエストパラメータを<code>RequestParam</code>でもらえる <br>
RequestParam("username")は request.getParameter("username")と考えよう

- **パラメータ必須**
> required = true //false

- **基本値**
> defaultValue = "値"

<code>@RequestMapping @GetMapping, @PostMapping</code>
```java
@RequestMapping(value = "/new-form", method = RequestMethod.GET)
```


---
出所 :
https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard
