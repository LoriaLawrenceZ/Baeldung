# **1. Overview**

JUnit is one of the most popular unit-testing frameworks in the Java ecosystem. The JUnit 5 version contains a number of exciting innovations, with **the goal of supporting new features in Java 8 and above**, as well as enabling many different styles of testing.

---

# **2. Maven Dependencies**

Setting up to **JUnit5.x.0** is pretty straightfoward: we just need to add th following dependency to out pom.xml:


~~~Java
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.9.2</version>
    <scope>test</scope>
</dependency>
~~~

Furthermore, there’s now direct support to run Unit tests on the JUnit Platform in Eclipse, as well as IntelliJ. We can, of course, also run tests using the Maven Test goal.

On the other hand, IntelliJ supports JUnit 5 by default. Therefore, running JUnit 5 on IntelliJ is pretty easy. We simply Right click –> Run, or Ctrl-Shift-F10.

It’s important to note that this version **requires Java 8 to work.**

---

## **3. Architecture**

JUnit5 compromises several different modules from three different sub-projects

### **3.1. JUnit Plataform**

The platform is responsible for launching testing frameworks on the JVM. It defines a stable and powerful interface between JUnit and its clients, such as build tools.

The platform easily integrates clients with JUnit to discover and execute tests.

It also defines the TestEngine API for developing a testing framework that runs on the JUnit platform. By implementing a custom TestEngine, we can plug 3rd party testing libraries directly into JUnit.

### **3.2. JUnit Jupiter**

This module includes new programming and extension models for writing tests in JUnit5. New annotations in comparison to JUnit4 are:

- **@TestFactory** – denotes a method that’s a test factory for dynamic tests
- **@DisplayName** – defines a custom display name for a test class or a test method
- **@Nested** – denotes that the annotated class is a nested, non-static test class
- **@Tag** – declares tags for filtering tests
- **@ExtendWith** – registers custom extensions
- **@BeforeEach** – denotes that the annotated method will be executed before each test method (previously @Before)
- **@AfterEach** – denotes that the annotated method will be executed after each test method (previously @After)
- **@BeforeAll** – denotes that the annotated method will be executed before all test methods in the current class (previously @BeforeClass)
- **@AfterAll** – denotes that the annotated method will be executed after all test methods in the current class (previously @AfterClass)
- **@Disabled** – disables a test class or method (previously @Ignore)

### **3.3. JUnit Vintage**

JUnit Vintage supports running tests based on JUNit3 and JUNit4 on the JUnit5 plataform

---

## **4. Basic Annotations**

To discuss the new annotations, we divided this section into the following groups responsible for execution: before the tests, during the tests (optional), and after the tests.

### **4.1. @BeforeAll and @BeforeEach**

Below is an examplo of the simple code to be executed before the main test cases:


~~~Java
@BeforeAll
static void setup() {
    log.info("@BeforeAll - executes once before all test methods in this class");
}

@BeforeEach
void init() {
    log.info("@BeforeEach - executes before each test method in this class");
}
~~~

It’s important to note that the method with the @BeforeAll annotation needs to be static, otherwise the code won’t compile.

## **4.2. @DisplayName and @Disabled**

Now let's move to new test-optional methods:

~~~Java
@DisplayName("Single test successful")
@Test
void testSingleSuccessTest() {
    log.info("Success");
}

@Test
@Disabled("Not implemented yet")
void testShowSomething() {
}
~~~

As we can see, we can change the display name or disable the method with a comment, using new annotations.

## **4.3. @AfterEach and @AfterAll**
Finally, let’s discuss the methods connected to operations after test execution:

~~~Java
@AfterEach
void tearDown() {
    log.info("@AfterEach - executed after each test method.");
}

@AfterAll
static void done() {
    log.info("@AfterAll - executed after all test methods.");
}
~~~

Please note that the method with `@AfterAll` also needs to be a static method.

---

# **5. Assertions and Assumptions**

JUnit5 tries to take full advantage of the new features from Java 8, especially lambda expressions.


~~~Java
@Test
void lambdaExpressions() {
    List numbers = Arrays.asList(1, 2, 3);
    assertTrue(numbers.stream()
      .mapToInt(Integer::intValue)
      .sum() > 5, () -> "Sum should be greater than 5");
}
~~~

Although the example above is trivial, one advantage of using the lambda expression for the assertion message is that it’s lazily evaluated, which can save time and resources if the message construction is expensive.
It’s also now possible to group assertions with assertAll(), which will report any failed assertions within the group with a MultipleFailuresError:

~~~Java
@Test
 void groupAssertions() {
     int[] numbers = {0, 1, 2, 3, 4};
     assertAll("numbers",
         () -> assertEquals(numbers[0], 1),
         () -> assertEquals(numbers[3], 3),
         () -> assertEquals(numbers[4], 1)
     );
 }
~~~

This means it’s now safer to make more complex assertions, as we’ll be able to pinpoint the exact location of any failure.