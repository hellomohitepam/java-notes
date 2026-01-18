# Wrapper Classes

[cite_start]Java provides wrapper classes around primitives so they can be used as classes and objects[cite: 21, 22]. [cite_start]All wrapper classes are present inside the `java.lang` package[cite: 25].

* [cite_start]**Boxing:** The process of wrapping is also known as boxing[cite: 24].
* [cite_start]**AutoBoxing:** The automatic conversion by the Java compiler between primitive types and their corresponding object wrapper classes (e.g., `int` to `Integer`)[cite: 61, 62, 63].
* [cite_start]**AutoUnBoxing:** The conversion from an object wrapper back to a primitive type[cite: 64].

## The Number Class
The `Number`, `Character`, and `Boolean` classes are child classes of the `Object` class[cite: 26]. The `Number` class has the following methods:
* `byteValue()`: Returns value as a byte[cite: 28].
* `doubleValue()`: Returns value as a double[cite: 29].
* `floatValue()`: Returns value as a float[cite: 30].
* `intValue()`: Returns value as an int[cite: 31].
* `longValue()`: Returns value as a long[cite: 33].
* `shortValue()`: Returns value as a short[cite: 34].

## Specific Wrapper Classes

### Integer
* [cite_start]Child class of `Number`[cite: 36].
* [cite_start]Wraps a primitive `int` in an object containing a single field of type `int`[cite: 37, 38].
* [cite_start]Inherits methods from the `Number` class[cite: 39].

### Byte
* [cite_start]Wraps a primitive `byte` in an object containing a single field of type `byte`[cite: 41, 42].

### Long
* [cite_start]Wraps a primitive `long` in an object containing a single field of type `long`[cite: 44, 45].

### Short
* [cite_start]Wraps a primitive `short` in an object containing a single field of type `short`[cite: 47, 48].

### Float
* [cite_start]Wraps a primitive `float` in an object containing a single field of type `float`[cite: 50, 51].

### Double
* [cite_start]Wraps a primitive value in an object (Note: The source text describes `float` types under the Double class section)[cite: 53, 54].

### Character
* [cite_start]Wraps a primitive `char` in an object containing a single field of type `char`[cite: 56, 57].

### Boolean
* [cite_start]Wraps a primitive `boolean` in an object containing a single field of type `boolean`[cite: 59, 60].
