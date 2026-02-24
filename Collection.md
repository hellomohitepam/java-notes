
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


















  
