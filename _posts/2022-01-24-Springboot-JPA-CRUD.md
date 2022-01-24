---
title:  SpringBoot　JPA　CRUD
excerpt: CRUD練習

categories:
  - SpringBoot
  - Jpa
tags:
  - CRUD

toc: true
toc_sticky: true
---

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder 
@Entity 
public class User {
	
	@Id 
	@GeneratedValue(strategy = GenerationType.IDENTITY) 
	private int id; 
	 
	@Column(nullable = false, length = 100, unique = true) 
	private String username; 
	
	@Column(nullable = false, length = 100) 
	private String password;
	
	@Column(nullable = false, length = 50)
	private String email; 

	@CreationTimestamp
	private Timestamp createDate;
}
```

```java
 //@Repository //自動でbeanに登録され省略可能 
public interface UserRepository extends JpaRepository<User, Integer>{
	
}
```
JpaRepository<User, Integer> : Userテーブルが管理するRepositoryでPrimaryKeyはinteger　　
- findALL  
- save : insert 、update  
- findById  
- count  
- deleteById



```java
@RestController
public class CrudControllerTest {

@Autowired 
private UserRepository userRepository;

@PostMapping("/crud/join")
public String join(User user) {         
	userRepository.save(user);
	return "会員加入が完了しました。";
	}

@GetMapping("/crud/user/{id}")
public User userDetail(@PathVariable int id) {
  User user = userRepositiory.findById(id).orElseThrow(()->{
    return new IllegalArgumentException("該当会員はいません。");
  });
}

@GetMapping("/crud/users")
public userList<User> list() {
  return UserRepository.findAll();
}

@Transactional
@PutMapping("/crud/user/{id}")
public User updateUser(@PathVariable int id, @RequestBody User requestUser) {
  User user = userRepositiory.findById(id).orElseThrow(()->{
    return new IllegalArgumentException("修正失敗しました。");
  });
  user.setPassword(requestUser.getPassword());
		user.setEmail(requestUser.getEmail());
    return user;
}

@DeleteMapping("/crud/user/{id}")
public String deleteUser(@PathVariable int id) {
  try {
    userRepository.deleteById(id);
  } catch(EmptyResultDataAccessException e) {
    return "作成に失敗しました。該当のidが存在しません。";
  }

  return "削除完了しました。";
}

}
```

---