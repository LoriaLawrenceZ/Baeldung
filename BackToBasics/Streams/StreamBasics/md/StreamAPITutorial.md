# **1. Overview**

In this comprehensive tutorial, we’ll go through the practical uses of Java 8 Streams from creation to parallel execution.

To understand this material, readers need to have a basic knowledge of Java 8 (lambda expressions, Optional, method references) and of the Stream API. In order to be more familiar with these topics, please take a look at our previous articles: [New Features in Java 8](https://www.baeldung.com/java-8-new-features) and [Introduction to Java 8 Streams](https://www.baeldung.com/java-8-streams-introduction).

# **2. Stream Creation**

There are many ways to create a stream instance of different sources. Once created, the instance will not modify its source, therefore allowing the creation of multiple instances from a single source.
