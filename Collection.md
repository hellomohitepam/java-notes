
## Collection
- A collection is simply an interface that represents a group of objects, known as its elements.

## Collection Framework
- It provides a set of interfaces and classes that help in managing groups of object.
> Before the introduction of the Collection Framework in JDK 1.2, Java used to rely on a variety of classes like `Vector`, `Stack`, `Hashtable`, and `arrays` to store and manipulate groups of objects.

# Collection vs Collections
| Feature         | `Collection`                                               | `Collections`                                                 |
| --------------- | ---------------------------------------------------------- | ------------------------------------------------------------- |
| Type            | **Interface**                                              | **Utility Class**                                             |
| Package         | `java.util`                                                | `java.util`                                                   |
| Purpose         | Represents a **group of objects** (data structure root)    | Provides **static utility methods** to operate on collections |
| Usage           | Used to create data structures like `List`, `Set`, `Queue` | Used for sorting, searching, synchronization, etc.            |
| Methods Type    | Instance methods                                           | Static helper methods                                         |
| Example Methods | `add()`, `remove()`, `size()`, `iterator()`                | `sort()`, `reverse()`, `shuffle()`, `min()`, `max()`          |
| Example Usage   | `Collection<String> c = new ArrayList<>();`                | `Collections.sort(list);`                                     |

## The Problems
* Inconsistency: Each class had a different way of managing collections, leading to confusion and a steep learning curve.
* Lack of inter-operability: These classes were not designed to work together seamlessly.
* No common interface: There was no common interface for all these classes, which meant you couldn't write generic algorithms that could operate on different types of collections.

## To solve these problems, the Collection Framework was introduced.
- Unified architecture: A consistent set of interfaces for all collections.
- Inter-operability: Collections can be easily interchanged and manipulated in a uniform way.
- Reusability: Generic algorithms can be written that work with any collection.
- Efficiency: The framework provides efficient algorithms for basic operations like searching, sorting, and manipulation.

## Key Interfaces in the Collection Framework
> The Collection Framework is primarily built around a set of interfaces. Important ones are:
- Collection: The root interface for all the other collection types.
- List: An ordered collection that can contain duplicate elements (e.g., ArrayList, LinkedList).
- Set: A collection that cannot contain duplicate elements (e.g., HashSet, TreeSet).
- Queue: A collection designed for holding elements prior to processing (e.g., PriorityQueue, LinkedList when used as a queue).
- Deque: A double-ended queue that allows insertion and removal from both ends (e.g., ArrayDeque).
- Map: An interface that represents a collection of key-value pairs (e.g., HashMap, TreeMap).


<img width="995" height="574" alt="image" src="https://github.com/user-attachments/assets/b9e2b3cb-6fd9-4915-aaea-ceb81b31e18e" />

- The Collection interface defines a set of core methods that are implemented by all classes that implement the interface. These methods allow for basic operations such as adding, removing, and checking the existence of elements in a collection.

# List
(a new array is created with a size 1.5 times the old array)
```
//java.util.Arrays$ArrayList
//java.util.ArrayList
Object[] array = list.toArray();
list.toArray(new Integer[0]);
```
## copyOnwriteArrayList
> Copy on Write means that whenever a write operation like adding or removing an element happen
> instead of directly modifying the existing list a new copy of the list is created, and the modification is applied to that copy
> This ensures that other threads reading the list while it’s being modified are unaffected.

- Read Operations: Fast and direct, since they happen on a stable list without interference from modifications.
- Write Operations: A new copy of the list is created for every modification.
- The reference to the list is then updated so that subsequent reads use this new list.
- when reads are more then write then use else not.

```java
       // List<String> shoppingList = new ArrayList<>(); // here .add() operation will give the error because while iterating it want stable ArrayList.
        List<String> shoppingList = new CopyOnWriteArrayList<>();
        shoppingList.add("Milk");
        shoppingList.add("Eggs");
        shoppingList.add("Bread");
        System.out.println("Initial Shopping List: " + shoppingList);

        for (String item : shoppingList) {
            System.out.println(item);
            // Try to modify the list while reading
            if (item.equals("Eggs")) {
                shoppingList.add("Butter");
                System.out.println("Added Butter while reading.");
            }
        }
```
---

# MAP
<img width="856" height="484" alt="image" src="https://github.com/user-attachments/assets/613b0487-c0ce-479c-9e05-3ea44ee7b51e" />

> Primitives can't be stored directly in HashMap/HashSet — they get autoboxed to wrapper types (int → Integer, etc.)

# HashMap component
- key -> The identifier used to retrieve the value 
- value -> The data associated with the key
- Bucket -> A place where key-value pair are stored. (Think of buckets as cells in a list(array))
- A hash function is an algorithm that convert key to index(bucket location) for storage.

# How Data is Stored in HashMap
## Step 1: Hashing the Key
- First, the key is passed through a hash function to generate a unique hash code which helps determine where the key-value pair will be stored in the array (called a "bucket array").
## Step 2: Calculating the Index
- The hash code is then used to calculate an index in the array (bucket location) using:
> int index = hashCode % arraySize;
> The index decides which bucket will hold this key-value pair.
## Step 3: Storing in the Bucket
- The key-value pair is stored in the bucket at the calculated index. Each bucket can hold multiple key-value pairs.

# How HashMap Retrieves Data
> When we call get(key), the HashMap follows these steps:
- Hashing the Key: Similar to insertion, the key is hashed using the same hash function to calculate its hash code.
- Finding the Index: The hash code is used to find the index of the bucket where the key-value pair is stored.
- Searching in the Bucket: Once the correct bucket is found, it checks for the key in that bucket. If it finds the key, it returns the associated value.

# Handling Collisions
- Since different keys can generate the same index (called a collision).
- If multiple key-value pairs map to the same bucket, they are stored in a linked list inside the bucket.
> When a key-value pair is retrieved, the HashMap traverses the linked list, checking each key until it finds a match.

# HashMap Resizing (Rehashing)
- HashMap has an internal array size, default is 16. When the number of key-value pairs exceeds a certain load factor (default is 0.75), HashMap automatically resizes the array to hold more data. This process is called rehashing.
- During rehashing The array size is doubled.
1. All existing entries are rehashed (i.e., their positions are recalculated) and placed into the new array.
2. This ensures the HashMap continues to perform efficiently even as more data is added.

# SortedMap
- is an interface that extends Map and guarantees that the entries are sorted based on the keys, either in their natural ordering or by a specified Comparator.

# NavigableMap 
- extends SortedMap, providing more powerful navigation options such as finding the closest matching key or retrieving the map in reverse order.

# INTERNAL KNOWLEDGE
- in object we have hashcode(by playing memory address) & equals(by reference)
- for custom class write hashcode and equals
- only primitive and String do not go with the memory they go according to their rule.


```java
// True for access order
LinkedHashMap<String, Integer> linkedHashMap = new LinkedHashMap<>(11, 0.3f, true); // double linked list
for (Map.Entry<String, Integer> entry : linkedHashMap.entrySet()) {
       System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

```
getOrDefault("Mohit",0)
putIfPresent
```

# HastTable

- Hashtable is synchronized
- no null key or value
- Legacy Class, ConcurrentHashMap
- slower than HashMap
- only linked list in case of collision
- all methods are synchronized {get, put,}

# ConcurrentHashMap

## Java 7 --> segment based locking --> 16 segments --> smaller hashmaps
- Only the segment being written to or read from is locked
- read: do not require locking unless there is a write operation happening on the same segment
- write: lock

## java 8 --> no segmentation
- Compare-And-Swap approach --> no locking except resizing or collision
- Thread A last saw --> x = 45
- Thread A work --> x to 50
- if x is still 45, then change it to 50 else don't change and retry
- put --> index
- incremental increase

# MAP --> SORTED --> THREAD SAFE --> ConcurrentSkipListMap  

## SkipList 
- probabilistic data structure that allows for efficient search, insertion, and deletion operations.
- It is similar to a sorted linked list but with multiple layers that "skip" over portions of the list to provide faster access to elements.
> when we insert new element in the BST then lots of nodes reshuffle happens but in SkipList every level not gonna change its random

# ENUM Map
- array of size same as enum
- no hashing
- ordinal/index is used
- FASTER THAN HASHMAP
- MEMORY EFFICIENT 

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

Map<Day, String> map = new EnumMap<>(Day.class);

map.put(Day.TUESDAY, "Gym");
map.put(Day.MONDAY, "Walk");

String s = map.get(Day.TUESDAY);
System.out.println(map);

// Sorted according to ENUM
```
# ImmutableMap

```java
Map<String, Integer> map1 = new HashMap<>();
map1.put("A", 1);
map1.put("B", 2);

Map<String, Integer> map2 = Collections.unmodifiableMap(map1);

map2.put("C", 3); //throws exception

Map<String, Integer> map3 = Map.of("Shubham", 98, "Vivek", 89);
map3.put("Akshit", 88);
Map<String, Integer> map4 = Map.ofEntries(Map.entry("Akshit", 99), Map.entry("Vivek", 99)); //only 10 key-value pairs are allowed

Map<String, Integer> map4 = Map.ofEntries(Map.entry("Akshit",99),Map.entry("Vivek",99));
```

# Set
## The Catch — Iteration is NOT auto-safe
```java
// This is NOT thread-safe even with synchronizedSet ❌
for (String s : syncSet) {
    System.out.println(s);
}

// You must manually synchronize iteration ✅
synchronized (syncSet) {
    for (String s : syncSet) {
        System.out.println(s);
    }
}
```
# ConcurrentHashMap.newKeySet()
- It creates a Set backed by a ConcurrentHashMap — designed for high-concurrency scenarios.

```

## The Full Family Tree
```
HashSet        → backed by → HashMap
LinkedHashSet  → backed by → LinkedHashMap  (HashMap + doubly linked list)
TreeSet        → backed by → TreeMap        (Red-Black Tree, sorted order)
```



 






































  
