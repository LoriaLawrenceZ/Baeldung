# 1.Overview

# 2.Setup

# 3.Application Configuration

~~~Java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
~~~

***@SpringBootApplication*** is equivalent to *@Configuration*, *@EnableAutoConfiguration* and *@ComponentScan*.

On the application.properties

~~~
server.port=8081
~~~

`server.port` changes the server port from the default 8080 to 8081

# 4.Simple MVC View

Add a simple MVC view with Thymeleaf.

First, we need to add the spring-boot-starter-thymeleaf dependency to build.gradle:

~~~Java
dependecies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
}
~~~

That enables Thymeleaf by default. No extra configuration is necessary.

We can now configure it in our application.properties:

~~~Java
spring.thymeleaf.cache=false
spring.thymeleaf.enabled=true 
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
~~~

Next, we’ll define a simple controller and a basic home page with a welcome message:

~~~Java
@Controller
public class SimpleController {
    @Value("${spring.application.name}")
    String appName;

    @GetMapping("/")
    public String homePage(Model model) {
        model.addAttribute("appName", appName);
        return "home";
    }
}
~~~

Finally, here is our home.html:

~~~HTML
<html>
<head><title>Home Page</title></head>
<body>
<h1>Hello !</h1>
<p>Welcome to <span th:text="${appName}">Our App</span></p>
</body>
</html>
~~~

Note how we used a property we defined in our properties and then injected that so we can show it on our home page.