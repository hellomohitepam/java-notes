# Reflection API
- Reflection in Java is a feature that allows a program to inspect and manipulate classes, methods, fields, and constructors at runtime, even if they are private.
- Normally Java works at compile time, but reflection allows runtime analysis and modification of code structure.

# Important classes:
- `java.lang.reflect`
| Class       | Purpose                       |
| ----------- | ----------------------------- |
| Class       | Represents a class at runtime |
| Method      | Represents methods            |
| Field       | Represents variables          |
| Constructor | Represents constructors       |
| Modifier    | Access modifiers info         |

# Class Object
1. Using forName() - `Class c = Class.forName("Apple");`
2. Using .class - `Class c = Apple.class;`
3. Using getClass() - `Class c = a.getClass();`

> Reflection can break encapsulation.

```
Load Class
      ↓
Get Class Object
      ↓
Get Constructor / Field / Method
      ↓
Set Accessible (if private)
      ↓
Invoke / Access
```

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

class Apple {

    private void repair() {
        System.out.println("Repairing");
    }
}

public class Main {
    public static void main(String[] args) throws Exception {

        Class c = Class.forName("Product"); // Loads class dynamically.

        System.out.println(c.getConstructors().length);

        Constructor constructors[] = c.getConstructors();  // Returns public constructors only.
        Constructor constructors[] = c.getDeclaredConstructors();  // Returns All constructors (including private).

        for (Constructor constructor : constructors) {
            System.out.println(constructor);
        }

        System.out.println("==============================");

        Field fields[] = c.getDeclaredFields();

        for (Field field : fields) {
            System.out.println(field);
        }

        Field f = c.getDeclaredField("price");
        f.setAccessible(true);  // Access Private Field

        System.out.println("==============================");

        Method methods[] = c.getMethods();

        for (Method method : methods) {
            System.out.println(method);
        }

        Method m = c.getDeclaredMethod("repair");  // Get Specific Method
        Method m = c.getDeclaredMethod("add", int.class, int.class); // if parameter exist

       System.out.println("==============================");

	Apple a = (Apple) c.getDeclaredConstructor().newInstance();

        Method m = c.getDeclaredMethod("repair", null);  // Old Method (Deprecated)
        Apple a = (Apple) c.getDeclaredConstructor().newInstance();  // Modern Method (Recommended)
        m.setAccessible(true);

        m.invoke(apple);
    }
}
```

# Advantages of Reflection
| Advantage         | Explanation                       |
| ----------------- | --------------------------------- |
| Dynamic behavior  | Classes can be loaded dynamically |
| Framework support | Used in frameworks                |
| Testing           | Used in testing tools             |
| Debugging         | Helps analyze runtime objects     |

# Disadvantages of Reflection
| Disadvantage             | Explanation                           |
| ------------------------ | ------------------------------------- |
| Slower                   | Reflection is slower than normal code |
| Security issues          | Can access private members            |
| Complex                  | Harder to maintain                    |
| Compile-time safety lost | Errors occur at runtime               |

# Reflection Performance
- Reflection is 2–10x slower than normal method calls.
- Because:
  - JVM checks metadata
  - Security checks occur

# Key Reflection Methods Cheat Sheet
| Method                    | Purpose                |
| ------------------------- | ---------------------- |
| forName()                 | Load class             |
| getMethods()              | Public methods         |
| getDeclaredMethods()      | All methods            |
| getFields()               | Public fields          |
| getDeclaredFields()       | All fields             |
| getConstructors()         | Public constructors    |
| getDeclaredConstructors() | All constructors       |
| setAccessible(true)       | Access private members |
| invoke()                  | Execute method         |

































