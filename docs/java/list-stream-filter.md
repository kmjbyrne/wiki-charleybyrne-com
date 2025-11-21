---
title: Java List Stream Filter
category: Java
tags:
  - java
  - collection
  - streams
---

## Collection

Java stream filters allow for a more succinct approach to filtering collections.

A naive approach would look something like:

```java
List<String> collection = new ArrayList<String>();
collection.add("Ireland");
collection.add("Finland");
collection.add("France");
collection.add("Germany");
collection.add("Italy");
collection.add("Hungary");

// one could also remove, if original collection is not required to be immutable
List<String> countriesWithLand = new ArrayList<String>();
for (String c : collection) {
    if (c.contains("land")) {
        countriesWithLand.add(c);
    }
}
```

A neater approach using collection streams and a filter expression:

```java
List<String> collection = new ArrayList<String>();
collection.add("Ireland");
collection.add("Finland");
collection.add("France");
collection.add("Germany");
collection.add("Italy");
collection.add("Hungary");

List<String> countriesWithLand = collection
    .stream()
    .filter(c -> c.contains("land"))
    .collect(Collectors.toList());
```
