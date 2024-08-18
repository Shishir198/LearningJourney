<h1>Annotations in Java</h1>

-Annotations in Java are a powerful feature introduced in Java 5 that allows you to add metadata to your Java code. 
-This metadata provides data about the program but is not part of the program itself. 
-Annotations can be used by the compiler and runtime to generate code, enforce rules, or provide additional information.

-They are defined using the @interface keyword and can be applied to various elements of the code such as 
classes, methods, fields, parameters, and local variables.


<h2>Common Uses of Annotations</h2>

Compile-Time Checking:
Annotations can be used to provide additional information to the compiler, which can be used to generate warnings or errors.
For instance, the @Override annotation indicates that a method is intended to override a method in a superclass. 
If the method does not actually override any method, the compiler will generate an error.

    @Override  -----> This is annotation
    public String toString() {
        return "This is an overridden method.";
    }
Code Generation:
Annotations can be processed at compile-time or runtime to generate code or configuration files. 
For example, libraries like Lombok use annotations to automatically generate boilerplate code such as getters and setters.

    @Data -----> This is annotation
    public class Person {
        private String name;
        private int age;
    }
The @Data annotation from Lombok generates getters, setters, toString, equals, and hashCode methods.

Runtime Processing:
Some annotations are used by frameworks and libraries to perform operations at runtime. 
For example, in Java EE and Spring frameworks, annotations are often used to configure beans, inject dependencies, or handle transactions.

    @Service -----> This is annotation
    public class MyService {
        @Autowired
        private MyRepository myRepository;
    }

    
Documentation:
Annotations can be used to provide additional information for documentation purposes. 
For example, the @Deprecated annotation marks a method or class as obsolete, which can be useful for developers maintaining the code.

    @Deprecated -----> This is annotation
    public void oldMethod() {
        // This method is deprecated and should not be used.
    }


<h2>In which scenario we must think , We should use Annotations</h2>:

-You have various service methods in your application that should only be accessible by users with specific roles, 
  such as ADMIN or USER. 
  Problem : 
  Manually checking user roles in each method could lead to repetitive code and is prone to errors.

  Solution :
  Custom Annotations
  We can apply those annotation in every method where user role is required instead of checking on each method everytime.

  Example : 
  
    public class UserService {

        @RequiresRole("ADMIN")
        public void deleteUser(String userId) {
            // Code to delete user
        }
    
        @RequiresRole("USER")
        public void getUserProfile(String userId) {
            // Code to get user profile
        }
    }

We will see , how to create your custom annotations later in this article. For now , we understood where its useful.


There are two types of annotations : 
1)PreDefined Annotation (built-in annotations provided by the Java language)
      Further in 2 types
      ---> Used as metadata for annotations
      ---> Used on java code like classes , methods etc.
      
2)Custom Annotation



Some PreDefined Annotations : 
    - Deprecated
    - Override
    - SupressWarnings
    - FunctionalInterface
    - SafeVarargs
    - Target (Meta-annotation)
    - Retention (Meta-annotation)
    - Documented (Meta-annotation)
    - Inherited  (Meta-annotation)
    - Repeatable (Meta-annotation)


1. @Deprecated
Provides a way to indicate that a feature is outdated and may be removed in future versions.
It helps maintain code by preventing the use of obsolete APIs and encouraging developers to use updated alternatives.
Example:

        public class OldClass {
              @Deprecated
              public void oldMethod() {
                  // This method is deprecated and should not be used.
              }
            
            public void newMethod() {
                // Recommended method
            }
        }

2. @Override

-Helps the compiler verify that a method is correctly overriding a method from its superclass. 
-If the method in Child does not correctly override a method in Parent (e.g., due to a typo in method name or incorrect parameters), 
 the compiler will generate an error, helping catch such issues early.
We can summarise , we use just for checking purpose that whether current method is overriding the superclass method correctly or not.

Example:

    public class Parent {
        public void display() {
            System.out.println("Display from Parent");
        }
    }
    
    public class Child extends Parent {
        @Override
        public void display() {
            System.out.println("Display from Child");
        }
    }

3. @SuppressWarnings
-Instructs the compiler to suppress specific warnings during compilation.
-Reducing unnecessary warnings that do not affect the functionality and keeps code base clean.
-@SuppressWarnings can take one or more warning types as arguments, such as "unchecked", "deprecation", "rawtypes", etc.
 this helps in cases where you need to ignore specific compiler warnings but still ensure the code is otherwise clean.

Example:

    @SuppressWarnings("unchecked")
    public void processList() {
        List rawList = new ArrayList(); // Suppressing unchecked warning
        rawList.add("item");
        List<String> stringList = rawList; // Unsafe cast
    }

4. @FunctionalInterface
 -Indicates that an interface is intended to be a functional interface (i.e., it has exactly one abstract method).
 -Provides documentation and compiler support for lambda expressions and method references.
  Ensures that the interface adheres to the functional interface contract.

Example:

    @FunctionalInterface
    public interface MyFunctionalInterface {
    
    void singleAbstractMethod();
      
    // Default method (optional)
    default void defaultMethod() {
        System.out.println("Default method");
    }
    
    // Static method (optional)
    static void staticMethod() {
        System.out.println("Static method");
    }
    }

5. @SafeVarargs

- Suppresses warnings about the use of varargs (variable-length argument lists) in situations where type safety might be compromised.
- Used to indicate that a method or constructor using varargs does not perform unsafe operations on the varargs array.
- This helps to suppress unnecessary warnings when you know that your varargs usage is safe.

Example:

        public class SafeVarargsExample {

          @SafeVarargs
          public final <T> void printItems(T... items) {
              for (T item : items) {
                  System.out.println(item);
              }
          }
       }  

Meta Annotations : 
They provide information about how the annotations themselves should be treated by the compiler and runtime

1. @Target
Function:

-Specifies the types of Java elements to which an annotation can be applied.
-Restricts the use of an annotation to specific elements, such as methods, fields, classes, or parameters. 
  This ensures that annotations are only used in appropriate contexts.

        import java.lang.annotation.ElementType;
      import java.lang.annotation.Target;
      
      @Target(ElementType.METHOD) // Annotation can only be applied to methods
      public @interface MyMethodAnnotation {
          String value();
      }


2. @Retention
 - Defines whether an annotation is available at runtime, compile time, or in the source code only.
 - Controls the lifespan of an annotation, determining whether it is discarded by the compiler or retained for runtime processing or documentation.

  
            import java.lang.annotation.Retention;
            import java.lang.annotation.RetentionPolicy;
            
            @Retention(RetentionPolicy.RUNTIME) // Annotation is available at runtime
            public @interface MyRuntimeAnnotation {
                String value();
            }


3.@Documented
Indicates that an annotation should be included in the JavaDoc documentation for the annotated element.

4.@Inherited
-Indicates that an annotation declared on a superclass will be inherited by subclasses.
-If annotation is applied on parent class , then it will automatically be applied to its subclasses also ,without need to specify.
-Allows annotations to be automatically inherited by subclasses, reducing the need to reapply annotations to every subclass.

      import java.lang.annotation.ElementType;
      import java.lang.annotation.Inherited;
      import java.lang.annotation.Target;
      
      @Inherited
      @Target(ElementType.TYPE)
      public @interface MyInheritedAnnotation {
          String value();
      }
      
      @MyInheritedAnnotation("Base class annotation")
      public class BaseClass {
          // Base class implementation
      }
      
      public class SubClass extends BaseClass {
          // Subclass implementation
      }

-@MyInheritedAnnotation on BaseClass will also apply to SubClass, so the annotation is inherited by subclasses.


5.@Repeatable
-Allows an annotation to be applied multiple times to a single Java element.
-Facilitates the use of the same annotation type more than once on a single element,
  improving flexibility and allowing more complex configurations.
-

      import java.lang.annotation.Retention;
      import java.lang.annotation.RetentionPolicy;
      import java.lang.annotation.Repeatable;
      
      @Retention(RetentionPolicy.RUNTIME)
      @Repeatable(MyRepeatableAnnotations.class)
      public @interface MyRepeatableAnnotation {
          String value();
      }
      
      @Retention(RetentionPolicy.RUNTIME)
      public @interface MyRepeatableAnnotations {
          MyRepeatableAnnotation[] value();
      }

Usage : 

    public class Example {

        @MyRepeatableAnnotation("First annotation")
        @MyRepeatableAnnotation("Second annotation")
        public void myMethod() {
            // Method implementation
        }
    }


<h2>Custom Annotations</h2>

To create a custom annotation, you need to define it using the @interface keyword. 
You can also specify meta-annotations to control how your annotation behaves.

1. Basic Syntax:

        import java.lang.annotation.ElementType;
        import java.lang.annotation.Retention;
        import java.lang.annotation.RetentionPolicy;
        import java.lang.annotation.Target;
        
        @Retention(RetentionPolicy.RUNTIME) // Specifies that the annotation will be available at runtime
        @Target(ElementType.METHOD) // Specifies that this annotation can be applied to methods
        public @interface MyCustomAnnotation {
            String description() default "No description"; // Optional element with default value
            int level() default 1; // Optional element with default value
        }
   
        @Retention(RetentionPolicy.RUNTIME): Indicates that the annotation will be available for reflection at runtime.
        @Target(ElementType.METHOD): Specifies that this annotation can only be used on methods. Other options include ElementType.TYPE (for classes, interfaces), ElementType.FIELD, etc.
        description and level: These are elements of the annotation, which can have default values.

3. Apply the Custom Annotation
You can now use your custom annotation in your code. For example, apply it to methods or classes as needed.

        public class ExampleClass {
        
            @MyCustomAnnotation(description = "This is a custom annotated method", level = 2)
            public void myMethod() {
                System.out.println("Method with custom annotation");
            }
        }
4. Process the Custom Annotation
 To process the custom annotation, you typically use reflection.
 This allows you to inspect annotations at runtime and perform actions based on their presence and values.

Example of Annotation Processing:

      import java.lang.reflect.Method;
      
      public class AnnotationProcessor {
          public static void main(String[] args) {
              try {
                  Method method = ExampleClass.class.getMethod("myMethod");
                  if (method.isAnnotationPresent(MyCustomAnnotation.class)) {
                      MyCustomAnnotation annotation = method.getAnnotation(MyCustomAnnotation.class);
                      System.out.println("Description: " + annotation.description());
                      System.out.println("Level: " + annotation.level());
                  }
              } catch (NoSuchMethodException e) {
                  e.printStackTrace();
              }
          }
      }
Explanation:

method.isAnnotationPresent(MyCustomAnnotation.class): Checks if the myMethod method is annotated with @MyCustomAnnotation.
method.getAnnotation(MyCustomAnnotation.class): Retrieves the @MyCustomAnnotation instance associated with myMethod.



---------------------------------------------------------------------------------------------------------------------------------------------------------
To be added : Heap Pollution issue with varargs.

