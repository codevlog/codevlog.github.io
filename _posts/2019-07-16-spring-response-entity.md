---
layout: post
title: "[Spring] REST API 개발하기 1 - 처음부터 끝까지 간단하게"
author: "coalee"
date: '2019-07-16 15:00:00 +0900'
categories: [howto, spring, backend, rest, api]
typora-root-url: ..
---

## 스프링으로 REST API 개발

- 서비스를 개발해 API로 공개하는 것은 서버 개발의 기본이다.
- API 방식으로는 HTTP Method + URL의 방식을 활용하는 REST API가 사실상의 표준으로 통용되고 있다.
- Spring + MySQL을 활용해 REST API 개발하는 방법을 알아보자.
- 예시로 User 정보를 CRUD 하는 API 개발.
- 개발환경: STS 4, MySQL 8.0 Server, MySQL Workbench, Open JDK 11



## 개발 순서

1. Spring Boot Project 생성 + 설정하기
2. 자바 클래스 만들기
   1. VO
   2. Service
   3. Controller
   4. DAO
   5. Mapper
3. MySQL DB, Mapper.xml 만들기



## 0. Data Model, API Spec. 정하기

- 가장 먼저 어떤 데이터를 저장하고 어떻게 공개할 건지 정한다.

### Data Model

![postentity](/assets/img/springRestAPI/postentity.PNG)

### API Spec

| URL            | HTTP Method | Request Body  | Response          | Description        |
| -------------- | :---------: | ------------- | ----------------- | ------------------ |
| /api/user      |    POST     | user JSON     | { success: true } | 사용자 Create      |
| /api/user      |     GET     |               | array of users    | 사용자 리스트 Read |
| /api/user/{id} |     GET     |               | user detail       | 사용자 Read        |
| /api/user/{id} |     PUT     | updating JSON | { success: true } | 사용자 Update      |
| /api/user/{id} |   DELETE    |               | { success: true } | 사용자 Delete      |



## 1. Spring Boot Project 생성 + 설정하기

### 프로젝트 생성
1. [File] - [New] - [Spring Starter Project]

   - Build Tool은 Gradle로 설정해봤다. 원래는 Maven 썼었는데 크게 차이는 없는 것 같고 xml보다 보기 편리한 것 같아서.

   ![create](/assets/img/springRestAPI/create.png)



2. Next를 누르고 Dependencies는 MyBatis, MySQL Driver, DevTools, Web Starter 네개만 설정했다.

   ![dependencies](/assets/img/springRestAPI/dependencies.png)

   

3. Finish를 눌러서 설정 완료



### 프로젝트 세팅

#### 1. 폴더구조

![structure](/assets/img/springRestAPI/structure.PNG)

- `com.codev`: 기본으로 생성되는 폴더. 아래 2번에서 작업할 `...Application.java`파일이 위치하고 있다.
- `com.codev.config`: Datasource를 동적으로 설정하기 위한 `DataSourceConfig.java`와 ID/PW 등 접속 정보를 갖고 있는 `_secret.java`파일이 위치하고 있다.
- `com.codev.controller`: 컨트롤러
- `com.codev.service`: 서비스
- `com.codev.dao`: Data Access Object. 사실 없애고 서비스에서 바로 mapper로 연결해도 동작은 되는데.. 나중의 확장성을 생각해 만들어둔다
- `com.codev.vo`: Value Object
- `com.codev.mapper`: Mapper class 
- `/src/main/resources/mapper`: 실제 SQL문을 포함하는`*Mapper.xml` 위치



#### 2. `...Application.java` 수정

- DB 작업을 할 Mapper와 VO를 등록해줘야 한다.

- `@MapperScan(basePackages = {"com.codev.mapper"})` 어노테이션을 App.java 클래스에 추가한다.

- `SqlSessionFactory` Bean을 아래와 같이 생성하며 VO와 `*Mapper.xml`을 찾을 수 있도록 한다. 

  ```java
  @SpringBootApplication
  @MapperScan(basePackages = {"com.codev.mapper"})
  public class CodevBackendApplication {
  	public static void main(String[] args) {
  		SpringApplication.run(CodevBackendApplication.class, args);
  	}
  	
  	@Bean
  	public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
  		final SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
  		sessionFactory.setDataSource(dataSource);
  		sessionFactory.setTypeAliasesPackage("com.codev.vo");
  
  		PathMatchingResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
  		sessionFactory.setMapperLocations(resolver.getResources("classpath:mapper/*Mapper.xml"));
  		return sessionFactory.getObject();
  	}
  }
  ```



#### 3. `config/DataSourceConfig.java` 생성

- 아래와 같이 만든다. `_secret.java`는 DB 주소와 ID/PW 등 보안을 위해 캡슐화했다. `.gitignore`에 명시해 웹에 올라가지 않도록 한다.

  ```java
  @Configuration
  public class DataSourceConfig {
  	@SuppressWarnings("rawtypes")
  	@Bean
  	public DataSource getDataSource() {
  		DataSourceBuilder dsb = DataSourceBuilder.create();
  		dsb.driverClassName(_secret.DB_DRIVERCLASS);
  		dsb.url(_secret.DB_URL);
  		dsb.username(_secret.DB_USERNAME);
  		dsb.password(_secret.DB_PASSWORD);
  		return dsb.build();
  	}
  }
  ```

  ```java
  public interface _secret {
  	public static final String DB_DRIVERCLASS = "com.mysql.cj.jdbc.Driver";
  	public static final String DB_URL = "jdbc:mysql://HOSTNAME:3306/codev?serverTimezone=Asia/Seoul";
  	public static final String DB_USERNAME = "username";
  	public static final String DB_PASSWORD = "password";
  }
  ```



## 2. 자바 클래스 만들기

### `vo/Post.java`

```java
public class Post {
	private int id;
	private String user_email, title, body, written_date, image_url;
	private int views, likes;
	
	// Constructors
	
	// Getter & Setters
	// ...
}
```



### `controller/PostController.java`

```java
@RestController
@RequestMapping("/api/post")
public class PostController {
	@Autowired
	PostService pSvc;
	
	/* post API */
	@PostMapping({"/", ""})
	public Map<String, String> postPost(@RequestBody Post post) {
		pSvc.addPost(post);

		Map<String, String> resultMap = new HashMap<>();
		resultMap.put("result", "success");
		return resultMap;
	}
	
	@GetMapping({"/", ""})
	public List<Post> getPosts() {
		return pSvc.selectAll();
	}
    
    @GetMapping("/{id}")
	public Post getPost(@PathVariable String id) { /* ... */ }

	@PutMapping("/{id}")
	public Map<String, String> putPost(@PathVariable String id, @RequestBody Post post){ /* ... */ }
	
	@DeleteMapping("/{id}")
	public Map<String, String> deletePortfolio(@PathVariable String id) { /* ... */ }
}
```



### `service, dao, mapper interfaces`

```java
public interface PostService {
public interface PostDao {
public interface PostMapper {
	public Post selectById(String id);
	List<Post> selectAll();
	public void addPortfolio(Post post);
	public void updatePortfolio(String id, Post post);
	public void deletePortfolio(String id);
}
```



### `service/PostServiceImpl.java`

```java
@Service
public class PostServiceImpl implements PostService {
	@Autowired
	PostDao pDao;
    
    @Override
    //...
}
```



### `dao/PostDaoImpl.java`

```java
@Repository
public class PostDaoImpl implements PostDao {
	@Autowired
	PostMapper mapper;
    
    @Override
    //...
}
```



## 3. MySQL DB, Mapper.xml 만들기
#### MySQL Post Table
```sql
CREATE TABLE `posts` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_email` varchar(255) NOT NULL,
  `title` varchar(255) NOT NULL,
  `body` text NOT NULL,
  `written_date` date NOT NULL,
  `image_url` varchar(512) DEFAULT NULL,
  `views` int(11) DEFAULT '0',
  `likes` int(11) DEFAULT '0',
  PRIMARY KEY (`id`),
  # KEY `fk_posts_users1_idx` (`user_email`),
  # CONSTRAINT `fk_posts_users` FOREIGN KEY (`user_email`) REFERENCES `users` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```



#### `/src/main/resources/mapper/PostMapper.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
   PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
   "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.codev.mapper.PostMapper">
	<select id="selectById" resultType="Post" parameterType="String">
		select * from posts where id = #{ id }
	</select>
	<select id="selectAll" resultType="Post">
		select * from posts
	</select>
	<insert id="addPost">
		insert into posts 
			(title, body, user_email, written_date, image_url)
		values 
			(#{title}, #{body}, #{user_email}, sysdate(), #{image_url})
	</insert>
	<update id="updatePost">
		update posts set
			title = #{post.title}, 
			body = #{post.body}, 
			image_url = #{post.image_url}
		where id = #{id}
	</update>
	<delete id="deletePost" parameterType="String">
		delete from posts
		where id = #{id}
	</delete>
</mapper>
```



## 결과

- 실행 후 Chrome, Postman 등으로 확인해보자.
- CRUD 기능이 잘 동작하는 것을 확인할 수 있었다.
- 아래 예시는 포스트 리스트를 읽는 작업

![result](/assets/img/springRestAPI/result.PNG)

