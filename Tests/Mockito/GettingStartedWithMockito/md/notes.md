# **1. Overview**

This document will cover the following **annotations of the Mockito library:** @Mock, @Spy, @Captor and @InectMocks

---

# **2. Enable Mockito Annotations**

There are different ways to enable the use of annotations with Mockito tests.

## **2.1. MockitoJUnitRunner**

The first option we have is to **annotate the JUnit test with a MockitoJUnitRunner**

~~~Java
@ExtendWith(MockitoExtension.class)
public class MockitoAnnotationUnitTest {
    ...
}
~~~

## **2.2. MockitoAnnotations.openMocks()**

Alternatively, we can **enable Mockito annotations programmatically** by invoking MockitoAnnotations.openMocks():

~~~Java
@BeforeEach
public void init() {
    MockitoAnnotations.openMocks(this);
}
~~~

## **2.3. MockitoJUnit.rule()**

Lastly, we can use a **MockitoJUnit.rule():**

~~~Java
public class MockitoAnnotationsInitWithMockitoJUnitRuleUnitTest {

    @Rule
    public MockitoRule initRule = MockitoJUnit.rule();

    ...
}
~~~

In this case, we must remember to make our rule public.

---

# **3. @Mock Annotation**

The most widely used annotation in Mockito is @Mock. We can use @Mock to create and inject mocked instances without having to call Mockito.mock manually.
In the following example, we’ll create a mocked ArrayList manually without using the @Mock annotation:

~~~Java
@Test
public void whenNotUseMockAnnotation_thenCorrect() {
    List mockList = Mockito.mock(ArrayList.class);
    
    mockList.add("one");
    Mockito.verify(mockList).add("one");
    assertEquals(0, mockList.size());

    Mockito.when(mockList.size()).thenReturn(100);
    assertEquals(100, mockList.size());
}
~~~
