<h1><strong>1. Overview</strong></h1>
<p>JUnit is one of the most popular unit-testing frameworks in the Java ecosystem. The JUnit 5 version contains a number of exciting innovations, with <strong>the goal of supporting new features in Java 8 and above</strong>, as well as enabling many different styles of testing.</p>


<hr>


<h1><strong>2. Maven Dependencies</strong></h1>
<p>Setting up to <strong>JUnit5.x.0</strong> is pretty straightfoward: we just need to add th following dependency to out pom.xml:</p>

~~~Java
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-engine</artifactId>
    <version>5.9.2</version>
    <scope>test</scope>
</dependency>
~~~

<p>Furthermore, there’s now direct support to run Unit tests on the JUnit Platform in Eclipse, as well as IntelliJ. We can, of course, also run tests using the Maven Test goal.</p>
<p>On the other hand, IntelliJ supports JUnit 5 by default. Therefore, running JUnit 5 on IntelliJ is pretty easy. We simply Right click –> Run, or Ctrl-Shift-F10.</p>
<p>It’s important to note that this version <strong>requires Java 8 to work.</strong></p>


<hr>


<h1><strong>3. Architecture</strong></h1>
<p>JUnit5 compromises several different modules from three different sub-projects</p>

<h2><strong>3.1. JUnit Plataform</strong></h2>
<p>The platform is responsible for launching testing frameworks on the JVM. It defines a stable and powerful interface between JUnit and its clients, such as build tools.</p>
<p>The platform easily integrates clients with JUnit to discover and execute tests.</p>
<p>It also defines the TestEngine API for developing a testing framework that runs on the JUnit platform. By implementing a custom TestEngine, we can plug 3rd party testing libraries directly into JUnit.</p>

<h2><strong>3.2. JUnit Jupiter</strong></h2>
<p>This module includes new programming and extension models for writing tests in JUnit5. New annotations in comparison to JUnit4 are:</p>
<ul>
    <li><span><strong>@TestFactory</strong> – denotes a method that’s a test factory for dynamic tests</span></li>
    <li><span><strong>@DisplayName</strong> – defines a custom display name for a test class or a test method</span></li>
    <li><span><strong>@Nested</strong> – denotes that the annotated class is a nested, non-static test class</span></li>
    <li><span><strong>@Tag</strong> – declares tags for filtering tests</span></li>
    <li><span><strong>@ExtendWith</strong> – registers custom extensions</span></li>
    <li><span><strong>@BeforeEach</strong> – denotes that the annotated method will be executed before each test method (previously @Before)</span></li>
    <li><span><strong>@AfterEach</strong> – denotes that the annotated method will be executed after each test method (previously @After)</span></li>
    <li><span><strong>@BeforeAll</strong> – denotes that the annotated method will be executed before all test methods in the current class (previously @BeforeClass)</span></li>
    <li><span><strong>@AfterAll</strong> – denotes that the annotated method will be executed after all test methods in the current class (previously @AfterClass)</span></li>
    <li><span><strong>@Disabled</strong> – disables a test class or method (previously @Ignore)</span></li>
</ul>

<h2><strong>3.3. JUnit Vintage</strong></h2>
<p>JUnit Vintage supports running tests based on JUNit3 and JUNit4 on the JUnit5 plataform</p>


<hr>


<h1><strong>4. Basic Annotations</strong></h1>
<p>To discuss the new annotations, we divided this section into the following groups responsible for execution: before the tests, during the tests (optional), and after the tests.</p>

<h2><strong>4.1. @BeforeAll and @BeforeEach</strong></h2>
<p>Below is an examplo of the simple code to be executed before the main test cases:</p>

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

<p>It’s important to note that the method with the @BeforeAll annotation needs to be static, otherwise the code won’t compile.</p>

<h2><strong>4.2. @DisplayName and @Disabled</strong></h2>
<p>Now let's move to new test-optional methods:</p>

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

<p>As we can see, we can change the display name or disable the method with a comment, using new annotations.</p>

<h2><strong>4.3. @AfterEach and @AfterAll</strong></h2>
<p>Finally, let’s discuss the methods connected to operations after test execution:</p>

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

<p>Please note that the method with @AfterAll also needs to be a static method.</p>


<hr>


<h1><strong>5. Assertions and Assumptions</h1></strong>
<p>JUnit5 tries to take full advantage of the new features from Java 8, especially lambda expressions.</p>

~~~Java
@Test
void lambdaExpressions() {
    List numbers = Arrays.asList(1, 2, 3);
    assertTrue(numbers.stream()
      .mapToInt(Integer::intValue)
      .sum() > 5, () -> "Sum should be greater than 5");
}
~~~

<p>Although the example above is trivial, one advantage of using the lambda expression for the assertion message is that it’s lazily evaluated, which can save time and resources if the message construction is expensive.</p>
<p>It’s also now possible to group assertions with assertAll(), which will report any failed assertions within the group with a MultipleFailuresError:</p>

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

<p>This means it’s now safer to make more complex assertions, as we’ll be able to pinpoint the exact location of any failure.</p>

<h2><strong></strong></h2>

<h2><strong></strong></h2>

<h2><strong></strong></h2>

<h2><strong></strong></h2>

<h2><strong></strong></h2>

<h2><strong></strong></h2>

<h2><strong></strong></h2>