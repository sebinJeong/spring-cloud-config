# spring-cloud-config

## 1. GitHub Public Repository URL

https://github.com/sebinJeong/spring-cloud-config.git

## 2. 실행 화면 캡처

- Eureka Dashboard
<img width="1313" height="1011" alt="스크린샷 2026-07-28 092605" src="https://github.com/user-attachments/assets/0cecd05d-e54d-44b6-a53e-0ea611c40122" />

    
- RabbitMQ Management



- Config Server

<img width="863" height="544" alt="스크린샷 2026-07-28 102652" src="https://github.com/user-attachments/assets/54ef780e-79fd-4777-b2d4-0f10d621b7d7" />


## 3. Postman 실행 결과 (위에서 실행한 모든 Postman의 실행 결과)

### step3.

<img width="1032" height="693" alt="스크린샷 2026-07-28 103934" src="https://github.com/user-attachments/assets/ee68edd1-5978-4578-aaab-237e402e190c" />

변경후

<img width="792" height="501" alt="스크린샷 2026-07-28 104005" src="https://github.com/user-attachments/assets/af736573-e7cd-48dd-bd31-9f62496b2a7f" />


### step4.

- 회원가입

<img width="1072" height="683" alt="스크린샷 2026-07-28 104927" src="https://github.com/user-attachments/assets/6978da83-972a-4c97-9305-2fcc90a7c88f" />


- 로그인

<img width="1044" height="657" alt="스크린샷 2026-07-28 105355" src="https://github.com/user-attachments/assets/fe66395a-af4f-4c2a-896b-ac084a901aef" />


- 사용자 목록

<img width="1073" height="599" alt="스크린샷 2026-07-28 110438" src="https://github.com/user-attachments/assets/adcd651b-6976-46ff-b8ce-e9d5231e0c97" />


- 상품 목록

<img width="1048" height="737" alt="스크린샷 2026-07-28 110604" src="https://github.com/user-attachments/assets/2029a9ce-e0ae-4f78-b5db-5a37727ca053" />


- 주문 생성

<img width="1068" height="695" alt="스크린샷 2026-07-28 110811" src="https://github.com/user-attachments/assets/3759f3a4-c713-4ab2-8cc6-f72794f368ef" />


- 환경설정 조회

<img width="1024" height="519" alt="스크린샷 2026-07-28 111346" src="https://github.com/user-attachments/assets/b567e08d-2eeb-4351-af4f-fcd765485c2b" />


## 4. 변경한 Config 파일

[config-service/application.yml]

```
server:
  port: 8888spring:
  application:
    name: config-service
  rabbitmq:
    host: 127.0.0.1
    port: 5672username: guest
    password: guest
#  profiles:#    active:#      - nativeprofiles:
    active: git
  cloud:
    config:
      server:
        native:
          search-locations: file://${user.home}/Desktop/Work/native-file-repo
        git: #default#          uri: file:///Users/edowon/Desktop/Work/git-local-repo#          uri: file:///C:/work/git-local-repo#          default-label: masteruri: https://github.com/sebinJeong/spring-cloud-config.git
          default-label: main
#          username: [username]#          password: [password]management:
  endpoints:
    web:
      exposure:
        include:
          - refresh
          - health
          - beans
          - info
          - httpexchanges
          - busrefresh
```

## 5. 구현한 Controller 및 Service 코드

UserController.java

```
packagecom.example.userservice.controller;

importcom.example.userservice.dto.UserDto;
importcom.example.userservice.jpa.UserEntity;
importcom.example.userservice.service.UserService;
importcom.example.userservice.vo.Greeting;
importcom.example.userservice.vo.RequestUser;
importcom.example.userservice.vo.ResponseUser;
importjakarta.servlet.http.HttpServletRequest;
importjakarta.servlet.http.HttpServletResponse;
importlombok.extern.slf4j.Slf4j;
importorg.modelmapper.ModelMapper;
importorg.modelmapper.convention.MatchingStrategies;
importorg.springframework.beans.factory.annotation.Autowired;
importorg.springframework.core.env.Environment;
importorg.springframework.http.HttpStatus;
importorg.springframework.http.ResponseEntity;
importorg.springframework.web.bind.annotation.*;

importjava.util.ArrayList;
importjava.util.HashMap;
importjava.util.List;
importjava.util.Map;

@RestController//@RequestMapping("/user-service")@RequestMapping("/")
@Slf4jpublic classUserController {
 

    @GetMapping("/users/message")
    publicString getMessage() {
        returnString.format("It's Working in User Service"+ ", welcome message="+ env.getProperty("greeting.message"));
    }

    @GetMapping("/users/company")
    publicMap<String, String> getCompany() {
        Map<String, String> companyMap = newHashMap<>();

        // Environment객체를 통해 최신 Config값을 읽어옴companyMap.put("name", env.getProperty("company.name"));
        companyMap.put("ceo", env.getProperty("company.ceo"));

        returncompanyMap;
    }
}
```
