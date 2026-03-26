
Java 8 --> minimal code, functional programing

Java 8 --> lambda expression, Streams, Date & Time API

lambda expression is an anonymous function ( no name, no return type, no access modifier )

if we have to implement single method in the class then we can use lambda expression (e.g. run in Runable interface).

functional interface(Which have only one abstract methods rest can be default & static) use to hold lambda expression.

lambda expression implements only functional interface

# Predicate
Functional interface(Boolean valued function)

```java
//expression is treated as variable
//predicate just hold the condition in the variable

Predicate<Integer> isEven = x -> x % 2 == 0;
System.out.println(isEven.test(4));

Predicate<String> startsWithA = x->x.startsWith("M");
Predicate<String> endsWithT = x->x.endsWith("T");
System.out.println((startsWithA.and(endsWithT).test("Mohit")));
```

# Function
do the operation and return
```java
Function<Integer,Integer> doubleIt = x->2*x;
Function<Integer,Integer> tripleit = x->3*x;

System.out.println(doubleIt.andThen(tripleit).apply(5));
System.out.println(doubleIt.compose(tripleit).apply(5));

Identity method
Function<Object, Object> identity = Function.identity();
System.out.println(identity.apply(51));
```

# Consumer 
just do operation no return
```java
Consumer<Integer> consumer = x-> System.out.println(x);
consumer.accept(46);

List<Integer> list = Arrays.asList(1,2,3,4);
Consumer<List<Integer>> consumer1 = x ->
{
    for(int i : x)
    {
       System.out.println(i);
    }
};
consumer1.accept(list);
```

# Suplier
Just do the operation No input
```java
Supplier<String> supplier = ()-> "Hello Mohit";
System.out.println((supplier.get()));
```

```java
// combined example
Predicate<Integer> predicate = x -> x % 2 == 0;
Function<Integer, Integer> function = x -> x * x;
Consumer<Integer> consumer = x -> System.out.println(x);
Supplier<Integer> supplier = () -> 100;

if (predicate.test(supplier.get())) {
    consumer.accept(function.apply(supplier.get()));
}
```
```java
// BiPredicate, BiConsumer, BiFunction

 BiPredicate<Integer, Integer> isSumEven = (x, y) -> (x + y) % 2 == 0;
 System.out.println(isSumEven.test(5, 5));

 BiConsumer<Integer, String> biConsumer = (x, y) -> {
        System.out.println(x);
        System.out.println(y);
};

BiFunction<String, String, Integer> biFunction = (x, y) -> (x + y).length();
System.out.println(biFunction.apply("a", "bc"));

// UnaryOperator, BinaryOperator
UnaryOperator<Integer> a = x -> 2 * x;

BinaryOperator<Integer> b = (x, y) -> x + y;
```
Note: - we can pass the method as parameter by two way
1. Lambda
2. Method refernce 

# Method reference
use method without invoking & in place of lambda expression
```java
List<String> students = Arrays.asList("Ram", "Shyam", "Ghanshyam");
students.forEach(System.out::println);

```

# Constructor reference
```java
List<String> names = Arrays.asList("A", "B", "C");
List<MobilePhone> mobilePhoneList = names.stream().map(MobilePhone::new).collect(Collectors.toList());

class MobilePhone{
    String name;

    public MobilePhone(String name) {
        this.name = name;
    }
}
```
---
# Streams

# Key Characteristics of Streams
- Process collections of data in a **functional (lambda-based)** manner
- Use **declarative programming** (describe *what* to do, not *how* to do it)
- Do not store data; they **process data on demand**
- Can be processed **sequentially or in parallel**

## Benefits of Using Streams
- **Simplifies data processing**
- **Encourages functional programming**
- **Improves readability and maintainability**
- **Enables easy parallelism**

 
# Creating Streams

## Infinite streams

- a. Infinite stream (Java 8)
    - ```Stream.iterate(T seed, UnaryOperator<T> f)```

- b. Finite stream (Java 9+)
    - ```Stream.iterate(T seed, Predicate<T> hasNext, UnaryOperator<T> f)```
- c. Always infinite
    - ```Stream.generate(Supplier<T> s)```
---
# Terminal Operation

- List<Integer> list = Arrays.asList(1, 2, 3);

## reduce
> Combines elements to produce a single result
- Optional<T> reduce(BinaryOperator<T> accumulator)
- T reduce(T identity, BinaryOperator<T> accumulator)

## forEach

```java
void forEach(Consumer<? super T> action)

list.stream().forEach(x -> System.out.println(x));
```

3. reduce
 Combines elements to produce a single result
```
T reduce(T identity, BinaryOperator<T> accumulator)
Optional<T> reduce(BinaryOperator<T> accumulator)
```

```java
Optional<Integer> optionalInteger = list.stream().reduce(Integer::sum);
System.out.println(optionalInteger.get());
```
4. count

```java
long a = list.stream().filter(x -> x.startsWith("A")).distinct().count();
```

5. anyMatch, allMatch, noneMatch

```java
boolean anyMatch(Predicate<? super T> predicate)

boolean b = list.stream().anyMatch(x -> x % 2 == 0);
System.out.println(b);

boolean b1 = list.stream().allMatch(x -> x > 0);
System.out.println(b1);

boolean b2 = list.stream().noneMatch(x -> x < 0);
System.out.println(b2);
```

6. findFirst, findAny

```java
System.out.println(list.stream().findFirst().get());
System.out.println(list.stream().findAny().get());
```

7. toArray()

```java
Object[] array = Stream.of(1, 2, 3).toArray();
```

8. min / max

```java
System.out.println("max: " + Stream.of(2, 44, 69).max((o1, o2) -> o2 - o1));
System.out.println("min: " + Stream.of(2, 44, 69).min(Comparator.naturalOrder()));
```

9. forEachOrdered

```java
List<Integer> numbers0 = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

numbers0.parallelStream().forEach(System.out::println);   // 76910835421
numbers0.parallelStream().forEachOrdered(System.out::println);  //12345678910
```
Note: Streams cannot be reused after a terminal operation has been called

```java
Stream<String> stream = names.stream();
stream.forEach(System.out::println);
List<String> list1 = stream.map(String::toUpperCase).toList(); // exception
```

Statefull: which know about the other elements like sorted
stateless: which do not know about the other elements like map

# Parallel Stream
* A type of stream that enables parallel processing of elements
* Allowing multiple threads to process parts of the stream simultaneously
* This can significantly improve performance for large data sets
* workload is distributed across multiple threads

```java
public class ParallelStream {
    public static void main(String[] args) {

        long startTime = System.currentTimeMillis();
        List<Integer> list = Stream.iterate(1, x -> x + 1).limit(20000).toList();
        List<Long> factorialsList = list.stream().map(ParallelStream::factorial).toList();
        long endTime = System.currentTimeMillis();
        System.out.println("Time taken with sequential stream: " + (endTime - startTime) + " ms");

        startTime = System.currentTimeMillis();
        factorialsList = list.parallelStream().map(ParallelStream::factorial).toList();
//      factorialsList = list.parallelStream().map(ParallelStream::factorial).sequential().toList();
        endTime = System.currentTimeMillis();
        System.out.println("Time taken with parallel stream: " + (endTime - startTime) + " ms");

        // Parallel streams are most effective for CPU-intensive or large datasets where tasks are independent
        // They may add overhead for simple tasks or small datasets

        // Cumulative Sum
        // [1, 2, 3, 4, 5] --> [1, 3, 6, 10, 15]
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        AtomicInteger sum =  new AtomicInteger(0);   // to achieve thread safety as lambda expression execute in parallel 
        List<Integer> cumulativeSum = numbers.stream().sequential().map(sum::addAndGet).toList();
        System.out.println("Expected cumulative sum: [1, 3, 6, 10, 15]");
        System.out.println("Actual result with parallel stream: " + cumulativeSum);

    }
    private static long factorial(int n) {
        long result = 1;
        for (int i = 2; i <= n; i++) {
            result *= i;
        }
        return result;
    }
}
```

---
# Collector
* Collectors is a utility class (Like Arrays)
* provides a set of methods to create common collectors

` List<String> names = Arrays.asList("Alice", "Bob", "Charlie");`
` List<Integer> nums = Arrays.asList(1, 2, 2, 3, 4, 4, 5);`
1. Collecting to a List

```java
List<String> res = names.stream().collect(Collectors.toList());
```

2. Collecting to a Set

```java
Set<Integer> set = nums.stream().collect(Collectors.toSet());
```

3. Collecting to a Specific Collection

```java
ArrayDeque<String> collect = names.stream().collect(Collectors.toCollection(() -> new ArrayDeque<>()));
```

4. Joining Strings
Concatenates stream elements into a single String

```java
String concatenatedNames = names.stream().map(String::toUpperCase).collect(Collectors.joining(", "));
```

5. Summarizing Data
Generates statistical summary (count, sum, min, average, max)

```java
IntSummaryStatistics stats = nums.stream().collect(Collectors.summarizingInt(x -> x));

System.out.println("Count: " + stats.getCount());
System.out.println("Sum: " + stats.getSum());
System.out.println("Min: " + stats.getMin());
System.out.println("Average: " + stats.getAverage());
System.out.println("Max: " + stats.getMax());

```
`Double average = numbers.stream().collect(Collectors.averagingInt(x -> x));`

6. Counting Elements
```java
Long count = nums.stream().collect(Collectors.counting());
```

7. Grouping Elements
```java
// 1. Basic - groups into Map<K, List<T>>
Collectors.groupingBy(Function<? super T, ? extends K> classifier)

// 2. With downstream collector
// The second argument lets you further process each group:
Collectors.groupingBy(Function<? super T, ? extends K> classifier,
                      Collector<? super T, A, D> downstream)

// 3. With map factory + downstream collector
// By default, the result is a HashMap
Collectors.groupingBy(Function<? super T, ? extends K> classifier,
                      Supplier<M> mapFactory,
                      Collector<? super T, A, D> downstream)
```
```java
List<String> words = Arrays.asList("hello", "world", "java", "streams", "collecting");
        System.out.println(words.stream().collect(Collectors.groupingBy(String::length)));
// {4=[java], 5=[hello, world], 7=[streams], 10=[collecting]}

     System.out.println(words.stream().collect(Collectors.groupingBy(String::length, Collectors.joining(", "))));
// {4=java, 5=hello, world, 7=streams, 10=collecting}

     System.out.println(words.stream().collect(Collectors.groupingBy(String::length, Collectors.counting())));
// {4=1, 5=2, 7=1, 10=1}

        TreeMap<Integer, Long> treeMap = words.stream().collect(Collectors.groupingBy(String::length, TreeMap::new, Collectors.counting()));
        System.out.println(treeMap);
// {4=1, 5=2, 7=1, 10=1}
```
# Common Downstream Collectors Summary
| Downstream Result Type       | Purpose                | Result Type  | Description                       |
| ---------------------------- | ---------------------- | ------------ | --------------------------------- |
| `toList()`                   | Default grouping       | `List<T>`    | Collects elements into a list     |
| `counting()`                 | Count per group        | `Long`       | Counts number of elements         |
| `summingInt/Long/Double()`   | Sum a field            | `Number`     | Sums values of a numeric field    |
| `averagingInt/Long/Double()` | Average a field        | `Double`     | Computes average of values        |
| `joining()`                  | Concatenate strings    | `String`     | Joins elements into one string    |
| `toSet()`                    | Deduplicate per group  | `Set<T>`     | Collects unique elements          |
| `mapping()`                  | Transform then collect | Transformed  | Applies mapping before collecting |
| `groupingBy()`               | Multi-level grouping   | `Nested Map` | Performs hierarchical grouping    |

8. Partitioning Elements
- partitioningBy is a special case of grouping that splits stream elements into exactly two groups — true and false — based on a Predicate. It always returns a Map<Boolean, ...>.
```java
// 1. Basic - partitions into Map<Boolean, List<T>>
Collectors.partitioningBy(Predicate<? super T> predicate)

// 2. With downstream collector
Collectors.partitioningBy(Predicate<? super T> predicate,
                          Collector<? super T, A, D> downstream)
```
```java
System.out.println(words.stream().collect(Collectors.partitioningBy(x -> x.length() > 5)));
```

9. Mapping and Collecting
Applies a mapping function before collecting
```java
 System.out.println(words.stream().collect(Collectors.mapping(x -> x.toUpperCase(), Collectors.toList())));
```

10. toMap

```java
List<String> words2 = Arrays.asList("apple", "banana", "apple", "orange", "banana", "apple");
        System.out.println(words2.stream().collect(Collectors.toMap(k -> k, v -> 1, (x, y) -> x + y)));;
```

```java

        // Example 1: Collecting Names by Length
        List<String> l1 = Arrays.asList("Anna", "Bob", "Alexander", "Brian", "Alice");
        System.out.println(l1.stream().collect(Collectors.groupingBy(String::length)));

        // Example 2: Counting Word Occurrences
        String sentence = "hello world hello java world";
        System.out.println(Arrays.stream(sentence.split(" ")).collect(Collectors.groupingBy(x -> x, Collectors.counting())));

        // Example 3: Partitioning Even and Odd Numbers
        List<Integer> l2 = Arrays.asList(1, 2, 3, 4, 5, 6);
        System.out.println(l2.stream().collect(Collectors.partitioningBy(x -> x % 2 == 0)));

        // Example 4: Summing Values in a Map
        Map<String, Integer> items = new HashMap<>();
        items.put("Apple", 10);
        items.put("Banana", 20);
        items.put("Orange", 15);
        System.out.println(items.values().stream().reduce(Integer::sum));
        System.out.println(items.values().stream().collect(Collectors.summingInt(x -> x)));

        // Example 5: Creating a Map from Stream Elements
        List<String> fruits = Arrays.asList("Apple", "Banana", "Cherry");
        System.out.println(fruits.stream().collect(Collectors.toMap(x -> x.toUpperCase(), x -> x.length())));

```
# Primitive Stream
## 📊 Intermediate Operations on Primitive Streams
### ✅ Common to IntStream, LongStream, DoubleStream
| Operation  | Description                      | Example                      |
| ---------- | -------------------------------- | ---------------------------- |
| filter()   | Keep elements matching condition | `.filter(n -> n > 5)`        |
| map()      | Transform values                 | `.map(n -> n * 2)`           |
| mapToObj() | Convert to object stream         | `.mapToObj(n -> "N=" + n)`   |
| boxed()    | Convert to wrapper stream        | `.boxed()`                   |
| distinct() | Remove duplicates                | `.distinct()`                |
| sorted()   | Sort elements                    | `.sorted()`                  |
| peek()     | Debug / side action              | `.peek(System.out::println)` |
| limit()    | Take first N elements            | `.limit(5)`                  |
| skip()     | Skip first N                     | `.skip(3)`                   |


# ✅ Type Conversion Operations
| Operation        | Converts To  | Available On         |
| ---------------- | ------------ | -------------------- |
| mapToInt()       | IntStream    | Long/Double streams  |
| mapToLong()      | LongStream   | Int/Double streams   |
| mapToDouble()    | DoubleStream | Int/Long streams     |
| asLongStream()   | LongStream   | IntStream only       |
| asDoubleStream() | DoubleStream | IntStream/LongStream |

# ✅ Flat Mapping (Java 9+)
| Operation | Description               |
| --------- | ------------------------- |
| flatMap() | Map to stream and flatten |
```java
IntStream.of(1,2)
         .flatMap(n -> IntStream.range(0, n));
```
# Primitive streams avoid boxing:
- ✔ Faster
- ✔ Less memory
- ✔ Numeric operations supported








