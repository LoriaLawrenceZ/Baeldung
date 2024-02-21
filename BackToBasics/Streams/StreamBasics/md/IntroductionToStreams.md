# **1. Overview**

In this article, we’ll have a quick look at one of the major pieces of new functionality that Java 8 had added *– Stream*s.

We’ll explain what streams are about and showcase the creation and *basic stream* operations with simple examples.

# **2. Stream API**

One of the major new features in Java 8 is the introduction of the stream functionality – [java.util.stream](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/stream/package-summary.html) – which contains classes for processing sequences of elements.

The central API class is the [*Stream<T>*](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/*stream*/*Stream*.html). The following section will demonstrate how streams can be created using the existing data-provider sources.

## **2.1. Stream Creation**

Streams can be created from different element sources e.g. *collections* or arrays with the help of *stream()* and *of()* methods:

~~~Java
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);
stream = Stream.of("a", "b", "c");
~~~

A *stream()* default method is added to the *Collection* interface and allows creating a *Stream<T>* using any *collection* as an element source:

~~~Java
Stream<String> stream = list.stream();
~~~

## **2.2. Multi-threading With Streams**

Stream API also simplifies multithreading by providing the *parallelStream()* method that runs operations over the stream’s elements in parallel mode.

The code below allows us to run method *doWork()* in parallel for every element of the *stream*:

~~~Java
list.parallelStream().forEach(element -> doWork(element));
~~~

In the following section, we will introduce some of the *basic Stream* API operations.

# **3. Stream Operations**

There are many useful operations that can be performed on a *stream*.

They are divided into intermediate operations (return *Stream<T>*) and terminal operations (return a result of definite type). Intermediate operations allow chaining.

It’s also worth noting that operations on streams don’t change the source.

Here’s a quick example:

~~~Java
long count = list.stream().distinct().count();
~~~

So, the *distinct()* method represents an intermediate operation, which creates a new stream of unique elements of the previous *stream*. And the *count()* method is a terminal operation, which returns stream’s size.

## **3.1. Iterating**

Stream API helps to substitute for, for-each, and while loops. It allows concentrating on operation’s logic, but not on the iteration over the sequence of elements. For example:

~~~Java
for (String string : list) {
    if (string.contains("a")) {
        return true;
    }
}
~~~

This code can be changed just with one line of Java 8 code:

~~~Java
boolean isExist = list.stream().anyMatch(element -> element.contains("a"));
~~~

## **3.2. Filtering**

The *filter()* method allows us to pick a stream of elements that satisfy a predicate.

For example, consider the following list:

~~~Java
ArrayList<String> list = new ArrayList<>();
list.add("One");
list.add("OneAndOnly");
list.add("Derek");
list.add("Change");
list.add("factory");
list.add("justBefore");
list.add("Italy");
list.add("Italy");
list.add("Thursday");
list.add("");
list.add("");
~~~

The following code creates a *Stream<String>* of the List<String>, finds all elements of this stream which contain char “d”, and creates a new stream containing only the filtered elements:

~~~Java
Stream<String> stream = list.stream().filter(element -> element.contains("d"));
~~~

## **3.3. Mapping**

To convert elements of a Stream by applying a special function to them and to collect these new elements into a *Stream*, we can use the *map()* method:

~~~Java
List<String> uris = new ArrayList<>();
uris.add("C:\\My.txt");
Stream<Path> stream = uris.stream().map(uri -> Paths.get(uri));
~~~

So, the code above converts *Stream<String>* to the *Stream<Path>* by applying a specific lambda expression to every element of the initial *Stream*.

If you have a *stream* where every element contains its own sequence of elements and you want to create a *stream* of these inner elements, you should use the *flatMap()* method:

~~~Java
List<Detail> details = new ArrayList<>();
details.add(new Detail());
Stream<String> stream
  = details.stream().flatMap(detail -> detail.getParts().stream());
~~~

In this example, we have a list of elements of type Detail. The Detail class contains a field PARTS, which is a *List<String>*. With the help of the flatMap() method, every element from field PARTS will be extracted and added to the new resulting *stream*. After that, the initial *Stream<Detail>* will be lost.

## **3.4. Matching**

Stream API gives a handy set of instruments to validate elements of a sequence according to some predicate. To do this, one of the following methods can be used: *anyMatch()*, *allMatch()*, *noneMatch()*. Their names are self-explanatory. Those are terminal operations that return a boolean:

~~~Java
boolean isValid = list.stream().anyMatch(element -> element.contains("h")); // true
boolean isValidOne = list.stream().allMatch(element -> element.contains("h")); // false
boolean isValidTwo = list.stream().noneMatch(element -> element.contains("h")); // false
~~~

For empty streams, the *allMatch()* method with any given predicate will return true:

~~~Java
Stream.empty().allMatch(Objects::nonNull); // true
~~~

This is a sensible default, as we can’t find any element that doesn’t satisfy the predicate.

Similarly, the *anyMatch()* method always returns false for empty streams:

~~~Java
Stream.empty().anyMatch(Objects::nonNull); // false
~~~

Again, this is reasonable, as we can’t find an element satisfying this condition.

## **3.5. Reduction**

*Stream* API allows reducing a sequence of elements to some value according to a specified function with the help of the *reduce()* method of the type *Stream*. This method takes two parameters: first – start value, second – an accumulator function.

Imagine that you have a *List<Integer>* and you want to have a sum of all these elements and some initial Integer (in this example 23). So, you can run the following code and result will be 26 (23 + 1 + 1 + 1).

~~~Java:
List<Integer> integers = Arrays.asList(1, 1, 1);
Integer reduced = integers.stream().reduce(23, (a, b) -> a + b);
~~~

## **3.6. Collecting**

The reduction can also be provided by the *collect()* method of type *Stream*. This operation is very handy in case of converting a stream to a *Collection* or a Map and representing a stream in the form of a single string. There is a utility class Collectors which provide a solution for almost all typical collecting operations. For some, not trivial tasks, a custom Collector can be created.

~~~Java
List<String> resultList 
  = list.stream().map(element -> element.toUpperCase()).collect(Collectors.toList());
~~~

This code uses the terminal collect() operation to reduce a *Stream<String>* to the *List<String>*.

# **4. Conclusions**

In this article, we briefly touched upon Java streams — definitely one of the most interesting Java 8 features.

There are many more advanced examples of using Streams; the goal of this write-up was only to provide a quick and practical introduction to what you can start doing with the functionality and as a starting point for exploring and further learning.
