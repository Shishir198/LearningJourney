<h1>Reflection API</h1>


-Reflection is commonly used by programs which require the ability to view or modify the runtime behavior of applications running in the Java virtual machine.

-Provides flexibility by allowing code to inspect and manipulate classes, methods, and fields dynamically at runtime.

-Reflection is important since it lets you write programs that do not have to "know" everything at compile time, making them more dynamic, 
 since they can be tied together at runtime. The code can be written against known interfaces, but the actual classes to be used can be instantiated 
 using reflection from configuration files.

 -We can access private members of class too using reflection on runtime.


Example of Reflection in Java
Here’s a complete example of using reflection to dynamically access and manipulate a class:

Class to Reflect:

    public class Person {
        private String name;
        private int age;

      public Person(String name, int age) {
          this.name = name;
          this.age = age;
      }
  
      private void displayInfo() {
          System.out.println("Name: " + name + ", Age: " + age);
      }
    }
Reflection Example:

    import java.lang.reflect.Field;
    import java.lang.reflect.Method;
    
    public class ReflectionExample {
        public static void main(String[] args) {
            try {
                // Load the class
                Class<?> personClass = Class.forName("Person");

                // Create an instance
                Object personInstance = personClass.getDeclaredConstructor(String.class, int.class).newInstance("John", 30);
    
                // Access and modify private field
                Field nameField = personClass.getDeclaredField("name");
                nameField.setAccessible(true);
                System.out.println("Original Name: " + nameField.get(personInstance));
                nameField.set(personInstance, "Jane");
    
                // Invoke private method
                Method displayInfoMethod = personClass.getDeclaredMethod("displayInfo");
                displayInfoMethod.setAccessible(true);
                displayInfoMethod.invoke(personInstance);
    
                // Check the updated field value
                System.out.println("Updated Name: " + nameField.get(personInstance));
            } catch (Exception e) {
                e.printStackTrace();
            }
          }
    }

Explanation:

Loading the Class: We use Class.forName("Person") to get the Class object representing the Person class.

Creating an Instance: We use reflection to create a new instance of Person with a specific constructor.

Accessing and Modifying Private Field: We access the private name field, modify its value, and print it.

Invoking Private Method: We invoke the private displayInfo method to display the person’s information.




<h1>Real-Life Scenarios and Examples of Reflection</h1>

1)Frameworks and Libraries

Many frameworks and libraries use reflection to provide their functionality. For example:

Dependency Injection Frameworks: 
Libraries like Spring use reflection to inspect classes and inject dependencies into fields and constructors at runtime.

ORM (Object-Relational Mapping) Libraries: 
Hibernate uses reflection to map Java classes to database tables, dynamically reading field names and types.

2)Testing Frameworks
Testing frameworks like JUnit and Mockito use reflection to access private methods and fields or create mock objects.

3)Plugin Systems
Reflection is used in plugin systems where new plugins or modules can be loaded and instantiated dynamically at runtime.




<h1>Use Case: Dependency Injection</h1>
One of the most common use cases for reflection is dependency injection (DI). 
DI is a design pattern used to implement Inversion of Control (IoC) where the control of object creation and dependency management 
is transferred from the application code to a framework or container. 
This is particularly useful in large applications where managing object dependencies manually becomes complex and error-prone.

<h2>Scenario: Building a Dependency Injection Framework</h2>
Imagine you're building a lightweight dependency injection framework. 
Your goal is to automatically inject dependencies into classes based on annotations or configuration, without requiring explicit setup in the client code.

Here’s a simplified example of how reflection can be used to achieve dependency injection:

1. Define Your Components with Annotations

You start by defining your classes and marking dependencies with annotations:

    // Define an annotation for injection
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.FIELD)
    public @interface Inject {}
    
    // Define a service interface and its implementation
    public interface Service {
        void serve();
    }
    
    public class MyService implements Service {
        @Override
        public void serve() {
            System.out.println("Service is serving...");
        }
    }

// Define a class that depends on the service
    
        
        public class Client {
            @Inject
            private Service service;
            public void doSomething() {
                service.serve();
            }
        }
2. Implement the Dependency Injection Framework

You use reflection to scan classes, find annotated fields, and inject dependencies:


    import java.lang.reflect.Field;
    import java.util.HashMap;
    import java.util.Map;

    public class SimpleInjector {
        private final Map<Class<?>, Object> instances = new HashMap<>();

    // Register a class and create an instance
    public <T> void register(Class<T> clazz) throws Exception {
        T instance = clazz.getDeclaredConstructor().newInstance();
        instances.put(clazz, instance);
    }

    // Inject dependencies into fields
    public void injectDependencies(Object target) throws Exception {
        Class<?> clazz = target.getClass();
        for (Field field : clazz.getDeclaredFields()) {
            if (field.isAnnotationPresent(Inject.class)) {
                field.setAccessible(true);
                Class<?> fieldType = field.getType();
                Object dependency = instances.get(fieldType);
                if (dependency != null) {
                    field.set(target, dependency);
                }
            }
        }
    }
    }
3. Use the Framework

You create an instance of your framework, register your components, and perform dependency injection:

    public class Main {
        public static void main(String[] args) throws Exception {
            SimpleInjector injector = new SimpleInjector();
        
        // Register classes
        injector.register(MyService.class);
        injector.register(Client.class);
        
        // Create a client instance
        Client client = new Client();
        
        // Inject dependencies
        injector.injectDependencies(client);
        
        // Use the client, which now has its dependencies injected
        client.doSomething();
    }
}
Explanation of Reflection Usage:

Field Access: Reflection is used to access fields in the Client class that are annotated with @Inject.
Field Injection: The SimpleInjector class uses reflection to set the values of these fields with the appropriate instances.
Benefits:

Decoupling: Your Client class doesn’t need to know how to create or manage Service instances.
Flexibility: You can change or configure dependencies without modifying the client code.


<h2>How to know , in which scenarios we need to use reflection</h2>


1)Dynamic Class Loading and Instantiation

Use Case: When you need to load classes and create instances at runtime without knowing their names at compile time.

2)Framework Development

Use Case: When developing frameworks or libraries that need to interact with user-defined classes, methods, or fields dynamically.

3)Dynamic Method Invocation

Use Case: When you need to invoke methods dynamically at runtime, especially when method names or parameters are not known until runtime.

Example: Implementing a scripting engine or command execution system where commands are mapped to method calls dynamically.
    
    Method method = clazz.getMethod("executeCommand", String.class);
    method.invoke(instance, "commandArgument");




<h2>What is "Class" class which JVM creates at runtime</h2>


-The 'Class' class in Java is a key component of the reflection API and is automatically created by the JVM when a class is loaded. 

-It represents the runtime metadata of a class or interface, providing methods to inspect, create, and manipulate objects dynamically. 

-Through the Class class, you can obtain detailed information about the class's structure, instantiate objects, and interact with fields 
and methods in a flexible and dynamic manner.

-The Class class in Java is used to represent a class or an interface that has been loaded into the JVM.

Inspecting Class Metadata:

The Class object provides methods to inspect various details about the class, such as its name, its superclass, its interfaces, and its fields, methods, and constructors.

Example of Inspecting Metadata:

    
    Class<?> clazz = String.class;
    
    // Get class name
    String className = clazz.getName(); // java.lang.String
    
    // Get superclass
    Class<?> superClass = clazz.getSuperclass(); // java.lang.Object
    
    // Get implemented interfaces
    Class<?>[] interfaces = clazz.getInterfaces(); // [interface java.io.Serializable, interface java.lang.Comparable]
    
    // Get declared fields
    Field[] fields = clazz.getDeclaredFields();



Reflection and Manipulation:

You can use the Class object to create instances of the class, invoke methods, and access fields and constructors, even if they are private or protected.

Example of Creating an Instance and Invoking a Method:

    Class<?> clazz = Class.forName("java.lang.String");
    
    // Create a new instance using a constructor
    Constructor<?> constructor = clazz.getConstructor(String.class);
    Object instance = constructor.newInstance("Hello");
    
    // Invoke a method
    Method method = clazz.getMethod("length");
    int length = (Integer) method.invoke(instance); // Invoke length() on the instance


How to access and change the value of a Private field?

    // Get a private field
    Field field = clazz.getDeclaredField("privateFieldName");
    field.setAccessible(true); // Bypass the access control checks
    
    // Get the value of the field
    Object value = field.get(instance);
    
    // Set a new value to the field
    field.set(instance, newValue);


How Reflection breaks Singleton design pattern and how to resolve?
Reflection can break the Singleton design pattern by allowing multiple instances of a class to be created, even if the class is designed to allow only one. This can be done by accessing and invoking the private constructor.

Example:

    // Assuming Singleton class has a private constructor
    Singleton instance1 = Singleton.getInstance();
    Constructor<Singleton> constructor = Singleton.class.getDeclaredConstructor();
    constructor.setAccessible(true);
    Singleton instance2 = constructor.newInstance();
    
    // Now instance1 and instance2 are different instances

Resolution:

One common way to mitigate this is to use the readResolve method in your Singleton class. 
This method is called during deserialization, and you can ensure that the deserialized instance is the same as the singleton instance:

    private Object readResolve() {
        return getInstance();
    }

Reflection can bypass the Singleton pattern by allowing multiple instances to be created through the private constructor.

Modify the constructor to throw an exception if an instance already exists. This ensures that attempts to create multiple instances using reflection will fail.


    public class Singleton {
        private static Singleton instance;

    private Singleton() {
        if (instance != null) {
            throw new RuntimeException("Use getInstance() to get the single instance of this class.");
        }
    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
    }

Or USE ENUM.




