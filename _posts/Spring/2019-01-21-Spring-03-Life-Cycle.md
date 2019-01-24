# Life Cycle

- 스프링 컨네이너와 빈은 언제 생성되고 소멸되는가?

1. 스프링 컨테이너와 빈의 생명주기
2. 생명주기를 통해 무엇을 할 수 있을까?
3. 생성과 소멸 때 특정 동작 설정하는 2가지 방법

<br>



## 1. 스프링 컨테이너와 빈의 생명 주기

```java
// 메모리에 springContainer가 생성됨
// 동시에 빈 객체들이 생성되어 의존관계 형성
GenericXmlApplicationContext ctx = 
    new GenericXmlApplicationConText("classpath:appCtx.xml");
BookRegisterService bookRegisterSerivce =
    ctx.getBean("bookRegisterService", BookRegisterService.class);

// 자원을 해지하는 시점에 스프링 컨테이너와 빈이 소멸됨
ctx.close():

```

- 스프링 컨테이너와 빈 생성 시점은 동일함
- 스프링 컨테이너의 소멸 = 빈 객체들의 소멸 = 메모리에서 사라짐
- 즉, 스프링 컨테이너와 빈 객체의 생명주기는 동일함

<br><br>

## 2. 이런 속성을 어떻게 활용할까?

- 생성과 소멸시점에 특정 동작을 설정할 수 있음
- `InitializingBean`과 `DisposalBean` interface를 구현해 설정할 수 있음
- 대게 DB연결을 위해 계정 ID/PW를 통해 인증을 받는 작업과 소멸시 자원을 회수하기 위한 작업에서 필요함

<br><br>

## 3. 생성/소멸시 특정 동작을 설정하는 2가지 방법

1. **Interface  사용**

   *BookRegisterService.java*

   ```xml
   public class BookRegisterService implements InitializingBean, DisposalBean {
   	
   	@Autowired
   	private BookDao bookDao;
   
   	public BookRegisterService() {}
   	
   	public void registerBook(Book book) {
   		bookDao.insert(book);
   	}
   
   	@Override
   	public void destroy() throws Exception {
   		System.out.println("객체 소멸");
   	}
   
   	@Override
   	public void afterPropertiesSet() throws Exception {
   		System.out.println("객체 생성");
   	}
   
   }
       
   ```

   - `InitializingBean`, `DisposableBean` 인터페이스를 사용해 객체가 생성될 때와 소멸될 때 실행할 동작을 설정할 수 있음

<br>

2. **init-method와 destroy-method**

   *BookSearchService.java*

   ```java
   public class BookSearchService {
   	
   	@Autowired
   	private BookDao bookDao;
   
   	public BookSearchService() {}
   	
   	public Book searchBook(String bNum) {
   		Book book = bookDao.select(bNum);
           return book;
   	}
   
   	public void destroyMethod() {
   		System.out.println("객체 소멸");
   	}
   
   	public void initMethod() {
   		System.out.println("객체 생성");
   	}
   
   }
   ```

   *appCtx.xml*

   ```xml
   <bean id="bookSearchService" class="com.brms.book.service.BookSearchService" 
   	init-method="initMethod" destroy-method="destroyMethod"/>
   ```


