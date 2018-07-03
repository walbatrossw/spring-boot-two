# [백기선님의 Spring Boot 2.0 레퍼런스 유투브 강의](https://www.youtube.com/watch?v=CnmTCMRTbxo&list=PLfI752FpVCS8tDT1QEYwcXmkKDz-_6nm3) 예제 코드 및 간단 요약 저장소 

**[Spring Boot Reference Guide](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#using-boot) 링크**

## DAY 01. 스프링부트 시작하기

### IntelliJ에서 스프링 부트 프로젝트 생성

- maven 프로젝트를 생성하고, `pom.xml`에 아래와 같이 작성한다.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>
    
        <groupId>com.example</groupId>
        <artifactId>myproject</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    
        <!-- Inherit defaults from Spring Boot -->
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.0.3.RELEASE</version>
        </parent>
    
        <!-- Add typical dependencies for a web application -->
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
        </dependencies>
    
    </project>
    ``` 

- IntelliJ 터미널에서 maven 빌드하기

    ```
    $ mvn package
    ```

    - 만약 빌드 실패할 경우 명령어를 수행하는 프로젝트 경로를 확인하고, 프로젝트 경로의 `pom.xml`이 있는 곳으로 이동하여 빌드를 다시 수행한다.

- `src/main/`에 패키지를 추가하고, Spring Boot 실행 클래스(`Example`)를 생성, 아래와 같이 코드를 작성해준다.

    ```java
    @RestController
    @EnableAutoConfiguration
    public class Example {
    
        @RequestMapping("/")
        String home() {
            return "hello world";
        }
    
        public static void main(String[] args) {
            SpringApplication.run(Example.class, args);
        }
    
    }
    ```
    
    - Stereotype annotation : `@RestController`, `@Controller`, `@Repository`, `@Service`, `@Component`
        - 이 클래스의 역할이 무엇인지를 코드를 읽는 사람에게 힌트를 준다.
        - 스프링이 이 클래스 역할이 무엇인지 인지하도록 하는 역할을 수행한다.
        
    - `@RequestMapping` : 라우팅 정보를 제공한다.  

    - `@EnableAutoConfiguration` : 스프링에게 어떤 설정을 원하는지 알려주는 애노테이션

- Spring MVC에 관련된 내용은 아래 링크를 통해 공부할 것.
    
    - [Spring Web MVC Reference](https://docs.spring.io/spring/docs/5.0.7.RELEASE/spring-framework-reference/web.html#mvc)


- 실행은 `main()` 메서드를 실행하거나 `$ mvn spring-boot:run` 명령어를 사용하면 된다.

- 종료는 `ctrl-c`이다.


---

## Day 02. Executable JAR 어떻께 만들고 어떻게 동작하는가?

### 스프링부트 프로젝트 배포하기 위한 Jar 패키지 빌드하기

- `pom.xml`에 아래와 같이 코드를 추가

    ```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
    ```
- maven 패키지 빌드

    ```
    $ mvn package
    ```
    
- `/target` 디렉토리에서 jar 파일 내용 확인

    ```
    $ jar tvf jar파일명.jar
    ```
    
- jar 파일 실행, 정상적으로 터미널에 스프링부트가 실행되는지 확인

    ```
    $ java -jar jar파일명.jar 
    ```
    
### 스프링부트 사용하기(Build System)

- Build System
    - Dependency Management(의존성 관리)
        - 스프링 부트는 maven dependency에 따로 버전을 명시하지 않아도 된다. 스프링 부트가 알아서 최적의 버전을 사용한다.
        - 만약 스프링 부트 버전을 올린다면 스프링부트는 알아서 충돌없이 버전업해준다.
        - 물론 버전 명시도 가능하다. 
        - **하지만 Spring Framework 버전은 명시하지 않는 것이 좋다. 스프링부트와 프레임워크는 강력하게 의존되어 있기 때문에**
    - Maven(메이븐)
        - 상속받은`spring-boot-starter-parent`에 기본설정이 다 되어있다.
        - 자바는 1.8, 인코딩은 UTF-8, 그외에 기타 내용들을 확인할 수 있다.
        - `spring-boot-starter-parent`없이 사용도 가능한데 아래와 같이 `pom.xml`에 작성해주면 된다.
            ```xml
            <dependencyManagement>
            		<dependencies>
            		<dependency>
            			<!-- Import dependency management from Spring Boot -->
            			<groupId>org.springframework.boot</groupId>
            			<artifactId>spring-boot-dependencies</artifactId>
            			<version>2.0.3.RELEASE</version>
            			<type>pom</type>
            			<scope>import</scope>
            		</dependency>
            	</dependencies>
            </dependencyManagement>
            ```