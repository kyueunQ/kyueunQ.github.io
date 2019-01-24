# Spring

학습 개요

1. 스프링 프레임워크란?
   - 프레임워크란 무엇인가?

2. 스프링 프레임워크의 모듈
3. 

## Spring Framework

- 주요기능: DI, AOP, MVC, JDBC 등 (구조를 만들어가는 방법론)
- DI: 기능을 만들어 놓고 필요할 때마다 주입해서 사용
- AOP: 관점지향. 주요기능만 작업을 하며, 공통된 부분은 떼었다가 붙였다가 하는 기능

<br>

### Framework 란?

- 틀로서 대게의 기능들이 이미 추상적으로 구현되어져 있음
- 필요한 기능들을 가져와 효율적으로 프로그램을 제작할 수 있음

<br>

## Spring Framework Module

1. spring-core : 핵심 기능인 DI(Dependency Injection)와 IoC(Inversion of Control)  제공
2. spring-aop: 관점지향 프로그램, 공통적인 요소는 별도로 제작하여 탈부착 가능
3. spring-jdbc: 데이터베이스를 쉽게 다룰 수 있음
4. spring-tx: 트랜젝션을 제공
5. spring-webmvc: 스프링 MVC 구현 기능 

<br>

> 과거 위의 모듈을 사용하기 위해서는 해당 기능의 라이브러리를 직접 다운을 받아서 진행 중인 프로젝트에서 import해서 사용했었다.
>
> 현재는 진행 중인 프로젝트의 **XML파일**에 필요한 기능들을 명시만 해주면 프레임워크에서 자동으로 라이브러리를 다운받아 사용할 수 있게 해준다.

<br>

## Spring Container (IoC)

- 객체들(빈Bean)이 담겨있는 통이라고 생각하기
- 객체지향언어(OOP)를 사용하는 프레임워크로 객체를 생성해서 사용하는 원리와 유사하게 작동

1. XML문서를 통해 객체를 생성
2. 스프링 컨테이너는 이러한 객체(= 프레임워크에서는 빈(Bean))들을 담고 있는 큰 그릇 역할임
3. 개발자는 개발문서(자바문서)에서 필요할 때마다 빈을 꺼내서 사용함

<br>

###  스프링 프로젝트 생성

- *maven project*로 생성하기
  - `코딩 - 컴파일 - 빌드 - 프로그램 실행`의 과정에서 봤을 때 *maven*dms 빌드를 해주는 tool 
  - 생성시 `Group id`에는 현재 진행하는 프로젝트의 상위 프로젝트 명을 적고, `Artifact id` 에는 현재 프로젝트 명을 기입함

<br>

### maven project 구조 파악

- `pjt폴더>src>main>java` : 기능 구현 공간으로 .java 파일이 모여있음
- `pjt폴더>src>main>resource` : 스프링설정파일(.xml)과 property파일 등 프로젝트 진행을 서포트해주는 파일들로 구성

<br>

### pom.xml 파일은 뭘까요?

- spring은 모듈(라이브러리)를 지원해주는 프로그램으로 `pom.xml`을 통해 원격지에 존재하는 메인 repository의 라이브러리를 가져와 사용할 수 있게 해줌

*pom.xml*

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
		http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>Group id 적기</groupId>
	<artifactId>Artifact id 적기</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.1.0.RELEASE</version>
		</dependency>

	</dependencies>


	<build>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
					<encoding>utf-8</encoding>
				</configuration>
			</plugin>
		</plugins>
	</build>


</project>

```

- 위의 코드가 pom.xml의 기본 형식이며 Group id와 Artifact id만 변경해서 사용하면 됨

<br>

### 객체 생성을 할 필요가 없다? - .xml

- 기존에 `new 클래스명`처럼 할 필요가 없이 .xml을 통해 스프링 컨테이너인 *IoC*를 통해 필요한 객체들을 생성해두고 필요할 때마다 꺼내 사용할 수 있음
- **`.xml`은 객체를 생성해주는 공간!**

<br>

*src/main/resources>OOO.xml*

```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
 		http://www.springframework.org/schema/beans/spring-beans.xsd">
	
    // 위의 부분은 공통적으로 사용되는 부분으로 새 프로젝트 생성시 복사 붙여넣기 해서 사용하기
    // 아래 부분이 빈(객체)를 생성하는 부분으로 변경이 필요함
	<bean id="tWalk" class="testPjt.TransportationWalk" />
	
</beans>
```

- `class="패키지명.프로젝트명"`
- `new 클래스명` 을 사용할 필요없이 .xml파일에 명시하면 객체가 자동으로 생성되고 메모리에 로드 됨 (로드 된 곳을 '스프링 컨테이너' 라고 부름)

<br>

### 작동 프로세스

*src/main/java/MainClass.java*

```java
package testPjt;

import org.springframework.context.support.GenericXmlApplicationContext;

public class MainClass {
	
	public static void main(String[] args) {

// 		이전 방식
//		TransportationWalk tras = new TransportationWalk();
//		tras.move();
		
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext("classpath:applicationContext.xml");
		// 자원의 내용을 적어줌
		
		//수많은 비 중에서 id가 저렇고, 이런 클래스로 부터 가져오겠다
		TransportationWalk tw = ctx.getBean("tWalk", TransportationWalk.class);
		tw.move();
		
		ctx.close();
	
	}
}
```

- `new GenericXmlApplicationContext("classpath:applicationContext.xml")`
  - `classpath:` + 빈(객체)가 생성된 공간이 어디인지 언급
- `GenericApplicationContext`의 자식 클래스인 `GenericXmlApplicationContext`로 .xml 파일에 접근하도록 데이터 타입을 설정함

- `TransportationWalk tw = ctx.getBean("tWalk", TransportationWalk.class);`

  - ctx.getBean(".xml파일에서 정한 id", "해당 클래스명");

    : .xml에 수 많은 빈(객체) 중에서 어떤 것을 사용할 것인가를 명시해줌

<br>

위와 같이 이클립스 내에서 프로젝트를 생성해서 사용하는 방법이 있는가 하면,

로컬에서 .java, resources파일을 생성 → pom.xml 생성 → import 해서 사용하는 방법도 있다.

프로젝트마다 방식이 다를 수 있기에 이 방법 또한 알아두자

<br>

<br>



> JVM → API → JRE → JDK



