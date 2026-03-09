* Integer class overrides the equals method to check for value equality, not reference equality.



Of course! Here is the comparison of `Iterator` and `ListIterator` from the image, presented in a table format for clarity.

| Feature | Iterator | ListIterator |
| :--- | :--- | :--- |
| **Introduced** | Java 1.2 | Java 1.2 |
| **Applicability** | Can be applied to any Collection interface (like `List`, `Set`, `Queue`). | Can only be applied to interfaces that extend `List`. |
| **Traversal** | Supports single-direction (forward) traversal only. | Supports bidirectional (forward and backward) traversal. |
| **Operations** | Allows for reading and removing elements. | Allows for reading, removing, adding, and replacing elements. |
| **How to Obtain** | By calling the `iterator()` method on a collection object. | By calling the `listIterator()` method on a list object. |
| **Core Methods** | `hasNext()`, `next()`, `remove()` | `hasNext()`, `next()`, `hasPrevious()`, `previous()`, `add()`, `set()`, `remove()` |

# Collections (implement Iterable)
```
list.forEach(Consumer<T>)        // List, ArrayList, LinkedList
set.forEach()                    // Set, HashSet, TreeSet
queue.forEach()                  // Queue, PriorityQueue
deque.forEach()                  // Deque, ArrayDeque
map.forEach(BiConsumer<K,V>)     // e.g. map.forEach((k,v) -> System.out.println(k+v))
```
# Streams
```
stream.forEach()         // Stream<T>
intStream.forEach()      // IntStream
longStream.forEach()     // LongStream
doubleStream.forEach()   // DoubleStream
```
# Cannot use forEach directly on:
```
int[] arr = {1,2,3};
arr.forEach()  // ❌ arrays don't have forEach method

// Instead use:
Arrays.stream(arr).forEach()   // ✅
```



