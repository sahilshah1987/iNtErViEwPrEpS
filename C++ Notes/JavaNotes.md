Knowledge Base for Concepts related questions in programming interview
===============
##Java
[notes](https://docs.google.com/document/d/1msRFUdvg1z6iNmHqUUI_KVXEWLtODc2JaZH6w2o7o9k/edit?usp=sharing)

* Comparisons:
  * StringBuffer Vs StringBuilder
    * StringBufer is **synchronized**, while StringBuilder is not.
    * StringBuilder provides an API compatible with StringBuffer, but with no guarantee of synchronization. This class is designed for use as a drop-in replacement for StringBuffer in places where the string buffer was being used by a single thread (as is generally the case). Where possible, it is recommended that this class be used in preference to StringBuffer as it will be faster under most implementations.
  * Array Vs Linkedlist
    * Size: array is fixed size, while linkedlist is dynamic size
    * Iteration: array provides random access, while linkedlsit can only be iterated one by one
    * Operation: array provides O(n) deletion and insertion, while linkedlist provides O(1)
    * Address sapce: array is stored as an object with continuous address, while linkedlist has to allocate extra space for separate listnode.
  * Abstract Class Vs Interface
    * [Official Doc: Abstract Classes Compared to Interfaces](http://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)
    * An abstract Class is a class that can not be instantiated. e.g. AbstractMap 
    * A Java Interface type declares methods but does not provide their implementations. e.g. Comparable, Map<K,V>, List<T>
    * Abstract class differs from interface in an important way - they can have instance variable, and they have concrete methods and constructors
    * A class can implement multiple interfaces, while only extend one abstract class
    * All methods in an interface should be public (since they’re all abstract methods); while in an abstract class, you can have different access control to the methods
    * In an interface, no need to use the "abstract" keyword
    Which should you use, abstract classes or interfaces?

  * Consider using abstract classes if any of these statements apply to your situation:
    *You want to share code among several closely related classes.
    *You expect that classes that extend your abstract class have many common methods or fields, or require access modifiers other than public (such as protected and private).
    *You want to declare non-static or non-final fields. This enables you to define methods that can access and modify the state of the object to which they belong.
  * Consider using interfaces if any of these statements apply to your situation:
    *You expect that unrelated classes would implement your interface. For example, the interfaces Comparable and Cloneable are implemented by many unrelated classes.
    *You want to specify the behavior of a particular data type, but not concerned about who implements its behavior.
    *You want to take advantage of multiple inheritance of type.

    *An example of an abstract class in the JDK is AbstractMap, which is part of the Collections Framework. Its subclasses (which include HashMap, TreeMap, and ConcurrentHashMap) share many methods (including get, put, isEmpty, containsKey, and containsValue) that AbstractMap defines.

    *An example of a class in the JDK that implements several interfaces is HashMap, which implements the interfaces Serializable, Cloneable, and Map<K, V>. By reading this list of interfaces, you can infer that an instance of HashMap (regardless of the developer or company who implemented the class) can be cloned, is serializable (which means that it can be converted into a byte stream; see the section Serializable Objects), and has the functionality of a map. In addition, the Map<K, V> interface has been enhanced with many default methods such as merge and forEach that older classes that have implemented this interface do not have to define.
  * Method Overloading Vs Override
    * Method overriding is a **run-time** phenomenon that is the **driving force behind polymorphism**. Implement the inherited method in a different way
      * same signature as the inherited method
      * happens at run time
    * Method overloading is a **compile-time** phenomenon. There’re two or more methods in the class that has the same method name but different parameters
      * Conditions for method overloading:
        * number of parameters are different in two methods
        * parameter type are different
      * Unqualified for method overloading:
        * change return type
        * change parameter name
      * Overloading happens at compile time, compiler determines whether a given method is correctly overloaded, if not, error

* String
  * String pool: comes from the idea that all already defined string are stored in same 'pool'(heap) and before creating String using "", compiler checks if such string is already defined. This is called [string interning](http://www.programcreek.com/2014/03/create-java-string-by-double-quotes-vs-by-constructor/)
  <pre><code>String s = "a" + "bc";
    String t = "ab" + "c";
    System.out.println(s == t); // true, have same reference.
    System.out.println(s.equals(t)); // true, have same value

    String c = "abcd";
    String d = new String("abcd");
    System.out.println(c == d);  // False, new String() create a new object
    System.out.println(c.equals(d)); // True
  </pre></code>
  * String immutability
    * Immutable objects cannot be modified once it's created. Any modification in immutable objects result in new objects
    * Why String is immutable in Java?
      * String pool, one obejct may have several references. If String is mutable, modify the object will influence all the references.
  * Substring
    * in JDK6, T = S.substring() reference to the original String object S. This will easily cause memory leak. Why? GC will not collect S even when it's not used, because T has a reference to it.
    * in JDK7, T = S.substring() will create a new String object. Thus solve this issue.

* Pass By Value
  * Java Passes everything by values
  * Objects and arrays are passed by values. The value here is a variable storing the memory address of the object/array (In short, value stores the reference to object)

* Private Constructor
  * use cases: Singleton pattern, enum

* Stack & Heap
  * Stack stores local variables. Deep recursion may cause StackOverFlow exception
  * Heap stores global variables. May cause OutOfMemory exception
  * In jave, size of stack is smaller than size of heap, although both can be adjusted. Thus, it's a good habit to put large space object on heap
  * Variables stored in stacks are only visible to the owner Thread, while objects created in heap are visible to all thread.

* Synchronized
  * two basic synchronization idioms: synchronized methods and synchronized statements
  * synchronized methods
  <pre><code>public class SynchronizedCounter {
      private int c = 0;

      public synchronized void increment() {
          c++;
      }

      public synchronized void decrement() {
          c--;
      }

      public synchronized int value() {
          return c;
      }
  }</code></pre>
  * synchronized statements
    * synchronized statements must specify the object that provides the intrinsic lock
  <pre><code>public class MsLunch {
      private long c1 = 0;
      private long c2 = 0;
      private Object lock1 = new Object();
      private Object lock2 = new Object();

      public void inc1() {
          synchronized(lock1) {
              c1++;
          }
      }

      public void inc2() {
          synchronized(lock2) {
              c2++;
          }
      }
  }</code></pre>


* TryCatchFinally
  * The finally block always executes when the try block exits.
  * the finally block is a key tool for preventing resource leaks
  * Case that finally will execute:
     * an unexpected exception occurs.  
     * return, continue, or break in try and catch, finally code will execute before return
  * Case that finally will not execute:
     * if the JVM exits while the try or catch code is being executed
     * the thread executing the try or catch code is interrupted or killed
     * forever loop in try or catch

* Java has 8 primitive data types
    * type                                             | default value
    * byte: 2^8                                            | 0 
    * short: 2^16                                          | 0 
    * int:2^32                                             | 0    
    * long: 2^64                                           | 0L
    * float: single-precision 32bit floating point;        | 0.0f
    * double: double-precision 64bit floating point;       | 0.0d
    * boolean:                                             | false
    * char: 16-bit unicode, from \u0000 to \uffff (0-65535)| \u0000

* Operator Precedence
    * from highest to lowest, just some common ones, [see all](http://introcs.cs.princeton.edu/java/11precedence/)
      * [],()
      * ++,--
      * *, /
      * +, -
      * >>, <<
      * ==, !=

* 'Static' Keyword
    * static method or static instance variable is owned by the class, instead of a specific instance. In other words, static members are shared by all instances.
    * static method cannot be overriden (if you create a method with the same return type and signature, that's called method hiding)
    * static members cannot be accessed by a non-static context
    * Static class, used in nested class, have no reference to instance of the outer class. One use case is if a nested class is used in static method in the outer class, you have to declare the nested class as static

* Interface: 
  * an interface  is similar to a class, but there are several differences
  * all methods in an interface are **abstract**; that is , they have a name, parameters ,and a return type, but they don’t have an implementation.
  * all methods in an interface type are automatically **public**
  * all interface type does not have instance variables, but can have constants (public static final) (why? because instance variable is part of the implementation)
  * use the implements reserved word to indicate that a class implements an interface type.
  * an interface can be implemented by a class or be extended by another interface
  * interface is not part of the class hierarchy, although they work in combination with classes. It provides common features
  * rewrite an interface will cause classes implementing the old interface to break because they don’t implement it anymore. (a class that implements an interface must implement all the methods declared in the interface)

* Sort:
  * Arrays.sort: uses tuned quicksort, if array size is small (<7), use insertion sort; otherwise, use quicksort.
  * Collections.sort: convert the ArrayList to array. Arrays.sort() the array, and reset the values in ArrayList


* Nested Class
  * Nested classes are divided into two categories: static and non-static. Nested classes that are declared static are called **static nested classes**. Non-static nested classes are called **inner classes**.

  <pre><code> class OuterClass {
      ...
      static class StaticNestedClass {
          ...
      }
      class InnerClass {
          ...
      }
  }</code></pre>

  * Non-static nested classes (inner classes) have access to other members of the enclosing class, even if they are declared private. Static nested classes do not have access to other members of the enclosing class.
  * Compelling reasons for using nested classes include the following:
    * It is a way of logically grouping classes that are only used in one place: If a class is useful to only one other class, then it is logical to embed it in that class and keep the two together. Nesting such "helper classes" makes their package more streamlined.
    * It increases encapsulation: Consider two top-level classes, A and B, where B needs access to members of A that would otherwise be declared private. By hiding class B within class A, A's members can be declared private and B can access them. In addition, B itself can be hidden from the outside world.
    * It can lead to more readable and maintainable code: Nesting small classes within top-level classes places the code closer to where it is used.
  * when the OuterClass is compiled, two separate files will be generated:
    * OuterClass.class
    * OuterClass$InnerClass.class
  * How to create an inner class object from outside the outer class code
    * OuterClass.InnerClass inner = outClass.new InnerClass();

* Garbage Collector
  * Mark and Sweep algorithm
    * look up the heap memory
    * identify unused objects and delete them
    * unused objects are defined as unreferenced objects (there’s no part in the program that holds a pointer to that object)
  * Reference counting
  * Stop the execution of program while GC
  * Calls finalize() method in an object before the obejct is finally destroyed
  * Can select different GC

* Reference
  * Weak reference: GC will collect the weak reachable object in a GC cycle
  * Soft reference: GC only collect when there's memory limit and need to reclaim memory
  * Strong reference: defualt reference, GC will not collect the strong reachable onject if there's any strong reference to it.

* Autoboxing & Unboxing
  * Autoboxing: 
    *the automatic conversion that the Java compiler makes between the primitive types and their corresponding object wrapper classes
    * Classic use of autoboxing is adding primitive types into Collections
  * Unboxing: 
    * Converting an object of a wrapper type (Integer) to its corresponding primitive (int) value is called unboxing
    * Classic use of unboxing is when Wrapper class are doing arithmetic operations (>=, <=, >, <, not including ==)
  * Compiler uses valueOf() method to convert primitive data to Object and uses intValue(), doubleValue() etc to get primitive value from Object.
    * Integer.valueOf: Returns an Integer instance representing the specified int value. Uses cached obejcts when value>=-128 && value<=127; otherwise, create new Integer.
  <pre><code>int i1 = 1;
        int i2 = 1;
        System.out.println("i1==i2 : " + (i1 == i2)); // true
        // Example 2: equality operator mixing object and primitive
        Integer num1 = 1; // autoboxing
        int num2 = 1;
        System.out.println("num1 == num2 : " + (num1 == num2)); // true, num1 unboxing
        // Example 3: special case - arises due to autoboxing in Java
        Integer obj1 = 1; // autoboxing will call Integer.valueOf()
        Integer obj2 = 1; // same call to Integer.valueOf() will return same
                            // cached Object
        System.out.println("obj1 == obj2 : " + (obj1 == obj2)); // true
        // Example 4: equality operator - pure object comparison
        Integer one = new Integer(1); // no autoboxing
        Integer anotherOne = new Integer(1);
        System.out.println("one == anotherOne : " + (one == anotherOne)); // false
  </pre></code>


* Data Structure
  * Collection, the root interface in the collection hierarchy
    * issues: duplicates, order, synchronize
    * extends Iterable<E>
  * List, is an interface
    * an ordered collection
    * extends Collections<E>
  * Set, a collection that contains no duplicates
    * contain at most one null element
    * extends Collection<E>
  * Vector, a growable array of objects
    * extends AbstractList<E>, implements List<E>, RandomAccess, Cloneable
    * iterator, fail-fast, will throw ConcurrentModificationException if the vector is structurally modified at any time after the iterator is created
    * Synchronized
    * When the array is full, it has to resize to support insertion operation. Thus, the worst case for insertion is O(n)
  * Stack, a class
    * extends Vector<E>
  * Queue, an interface
    * extends Collection<E>
  * LinkedList
    * extends AbstractSequentialList<E>, implements List<E>, Deque<E>, Cloneable
    * permits null
    * can be used as a stack, queue, or double-ended queue
    * Unsynchronized, use Collections.synchronizedList(new LinkedList()), which add mutex to each method (synchronzied(lock);)
  * ArrayList
    * extends AbstractList<E>, implements List<E>, RandomAccess, Cloneable
    * Resizable-array implementation of the List interface.
    * permits null
    * roughly equivalent to Vector, except that it's unsynchronized
  * TreeMap
    * A red-black tree based NavigableMap implementation
    * lookup, get, put, remove, O(lgn)
    * not synchronized,  
    <pre><code>SortedMap m = Collections.synchronizedSortedMap(new TreeMap(...));</code></pre>
  * HashMap
    * provide quick access to key-value pairs
    * is basically an array of buckets, each bucket holds a list of entires
    * we calculate hash code from the key, and get the index to store key-value pairs
    * if two keys have the same hash code, they're mapped into the same bucket, and chained using linked list.
    * when we resize the hashmap, the hash code of each key remains the same, but they're mapped to different index in the array. This will take O(n).
    * In Java source code, hash code is int type, so we can have maximum 2^32 different hashcodes.
    * equals & hashcode
      * hashcode() in Object.class: return distinct integers for distinct objects. (This is typically implemented by converting the internal address of the object into an integer)
      * When we use our own created class as keys in hashmap, we need to overwrite the hashcode() method in Object.class. There're two contracts:
        * if two objects are equal(), they should have the same hashcode()
        * if two objets have the same hashcode(), they may be equal() or not equal
      * To get the hashcode of a primitive data type, we can use its wrapper class's hashcode.eg, Integer.valueOf(10).hashcode()
      * example of hashcode() in Java source code.
      <pre><code>// Integer.class
        public int hashcode(){
        return value;
      }
      // String.class
      // Returns a hash code for this string. The hash code for a String object is
      // computed as s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1] using int arithmetic,
      // where s[i] is the ith character of the string, n is the length of the string, 
      // and ^ indicates exponentiation
      * example of equals()
      <pre><code>@override
      public boolean equals(Object other){
        if (other == this)  
          return true;
        if (other==null || other.getClass()!=this.getClass())
          return flase;
        MyClass c = (MyClass) other;
        return this.value == c.value;
      }</pre></code>

  * BitSet
    * basically an array of long, if the array size is n, we can represent 64*n different integers
    * BitSet.set(int), BitSet only supports non negative integers.
    * If the integer set is sparse, BitSet will waste a lot of space. An example is an integer set of only one integer 1,000,000,000, this will take 2^30/64 = 16MB space. In this case, Set<Integer> is a better choice

* Generics
  * added in JDK5
  * Use case: Collections, generic method, generic class
  * Generic Type:
    * A generic type is a generic class or interface that is parameterized over types. eg, Comparator<T>
    * A type variable can be any non-primitive type (class type, interface type, array type...)
    * Declaration:
      *  class name\<T1, T2, ..., Tn>{}
    * Example:
    <pre><code>// Class without generic type
    public class Box{
      private Object obj;
      public void set(Object obj){this.obj = obj;}
      public Object get(){return obj;}
    }
    // Class with generic type
    public class Box\<T>{
      private T t;
      public void set(T t){this.t = t;}
      public T get(){return t;}
    }
    </pre></code>
  * Generic Method
    * Similar to generic class, but the scope of the type parameter is limited to the method.
    * Example
    <pre><code>// compare() in Comparator<T>
    @override
    public int compare(T o1, T o2)
    </pre></code>
  * In the following case, line 2 will fire a complie error. In general, if Foo is a subtype (subclass or subinterface) of Bar, and G is some generic type declaration, it is not the case that G<Foo> is a subtype of G<Bar>. This is probably the hardest thing you need to learn about generics, because it goes against our deeply held intuitions.
  <pre><code>List<String> ls = new ArrayList<String>(); // 1
    List<Object> lo = ls; // 2 
  </code></pre>
  * Bounded and Unbounded wildcards
    * So what is the supertype of all kinds of collections? It's written Collection<?> (pronounced "collection of unknown"), that is, a collection whose element type matches anything. It's called a **wildcard type**
    * List<? extends Shape> is an example of a **bounded wildcard**. drawAll() will accept lists of any subclass of Shape.
    <pre><code>public void drawAll(List<? extends Shape> shapes) {
      ...
    }
    </code></pre>
    That price to be paid for the flexibility of using wildcards is that it is now illegal to write into shapes in the body of the method. For instance, this is not allowed:
    <pre><code>public void addRectangle(List<? extends Shape> shapes) {
        shapes.add(0, new Rectangle()); // Compile-time error!
    }
    </code></pre>
  * Type Erasure: remove all type-related information at compile-time (remove code with generics). This means that generics in Java is syntactic sugar
  * Type Inference:Generics in Java does not support type inference while calling constructor or creating instance of Generic Types until JDK7
  <pre><code>//prior to JDK 7
  HashMap<String, Set<Integer>> contacts = new HashMap<String, Set<Integer>>();
  //JDK 7 diamond operator
  HashMap<String, Set<Integer>> contacts = new HashMap<>();
  </pre></code>
  * Ads:
    * Type-safety, provide clean and robust code
    * No casting when get element from the Collection 
  * Limitations:
    * Generics cannot be applied to primitive types(int), should use wrapper class(Integer)

* instanceof
  * the type comparison operator
  * (obj1 instanceof Parent)
  * for the following code, no error, the output is *Cat Kitty*
  <pre><code>public void testArrayListOfDifferentSubtypes() {
        ArrayList<Animal> an = new ArrayList<Animal>();
        Animal cat = new Cat("Kitty");
        Animal dog = new Dog("Wangwang");
        an.add(cat);
        an.add(dog);
        for (Animal a : an){
           if (a instanceof Cat)
              System.out.println(a.print());
        }
   }
   abstract class Animal {
        protected String name;

        public String print() {
           return name;
        }
   }
   class Cat extends Animal {
        public Cat(String str) {
           name = "Cat " + str;
        }
   }
   class Dog extends Animal {
        public Dog(String str) {
           name = "Dog " + str;
        }
   }   
  </pre></code>

##System & Scalability
[NEU-CS 6240: Parallel Data Processing in MapReduce](http://www.ccs.neu.edu/home/mirek/classes/2011-F-CS6240/)
[Nootcod3r: Count Occurences](http://n00tc0d3r.blogspot.com/2013/07/big-data-count-occurrences.html)<br>
* Given a large number (millons or billons) of records (integers, IPs, URLs, query key words, etc.),
  * find out top k most frequent records;
  * find out duplicates;
  * find out whether a given number x is in the set or not.
* LRUCache
  * single machine
  * multiple machine
* Social Graph
  * Facebook/LinkedIn connections
* Consistent hashing
  * a special kind of hashing such that when a hash table is resized, only K/n keys need to be remapped on average, where K is the number of keys, and n is the number of slots. In contrast, in most traditional hash tables, a change in the number of array slots causes nearly all keys to be remapped.
  * [Nootcod3r](http://n00tc0d3r.blogspot.com/2013/09/big-data-consistent-hashing.html)
  * The key point is that we change the one-one mapiing between N%k and server# to multi-one
* Log
  * append-only, totally-ordered sequence of records ordered by time
  * two-types: 
    * System and Application daemon logs
    * Event logs
      * Kafka -- Linkedin
        * publish-subscribe messaging rethought as a distributed commit log
      * Scribe -- Facebook

##Streaming Algorithm / Online Algorithm
* Streaming Algorithm  
  * algorithms for processing data streams in which the input is presented as a sequence of items and can be examined in only a few passes (typically just one). 
  * limited memory available to them (much less than the input size)
  * limited processing time per item


##Networking & Protocol
  [What Every Web Developer Should Know About HTTP (OdeToCode Programming Series) - Allen, K. Scott](http://www.amazon.com/Developer-Should-OdeToCode-Programming-Series-ebook/dp/B0076Z6VMI)
* URL -- Uniform Resource Locator
    * schema://host:port/path?query#frag
      * schema: http, https, ftp, etc
      * host: xxx.com
      * url path: /abc/def/109
      * port number: the host use it to listen for http request. The default value is 80
    * url may point to a file on host's file system or disk space (http://xxx.com/a.jpg).
    Yet, sometimes, it doesn't refer to any file on the server(although url is a 'locator'). In this case, when the appplication receives the http request, it will build a web page  by controlling views and model (MVC)
    * query: http://www.google.com/search?q=apple&p=banana
      * everything after ? is known as the query.
      * usually is a name/value pair a=b
    * fragment:http://xxx.com/apple#google
      * fragment is not processed by the server, only used on client-side to identify a specific section of the page (by element's id).
    * url encode: encode **unsafe character**(whiete sapce, ^, etc.) into safe character
      * 'my book ' -> 'my%20book'
* Http transaction
    * request/response
      * build network connection and send http message
    * content type (media type)
      * MIME (Multiple Internet Mail Extension) standards.
      * file extension is the last thing used to check actual content type
    * request type
      * Get/Post; Delete/Put/Head
      * Get, retrieve; Post, form, input
      * Post/Redirect/Get pattern (Post is unsafe, will modify data on the server; while Get is safe)
    * Http Request
      * [method][URL][version]
        [headers]
        [body]
      * in ASCII text
      * there're numerous headers defiend by the Http specification
    * Response Status Code (200/400/500, etc)
    * Response headers: to deal with cache and performance of the web
* State and Security
    * HTTP is a stateless protocol, how to maintain states on the website?
    * Cookie: HTTP State Management Mechanism
      * identify user
      * track user requests
      * cookie only stores a unique user identifier, the user information is stored on the server, not in cookies
      * the UID should be encrypted to avoid security concerns
      * Flags: **HttpOnly**
      * types: Session cookies (exist for a single user); Persistent cookies (needs an **Expires** value)
    * Set-Cookie:
      * name-value pairs
      * size limitation: 4KB
      * Path & Domain: controls the scope using domain and path attritbutes
        * domain: allows a cookie to span sub-domains (e.g., domain: server.com, sub-domain: pic.server.com, file.server.com, etc.)
        * path: restrict a cookie to a specific path.
    *Authentication
      * Basic Authentication -- base 64 encoding
        * encode password on client, and decode on server
      * Digest Authentication
        * Improvement over basic authentication
        * client send a digest of the password, using MD5 hashing
    *Secure HTTP -- HTTPS
      * default port is 443, instead of 80
      * SSL: additional security layer between HTTP and TCP
      * All trafic over HTTPS is encrypted in the request and response, including the HTTP headers and message body.
      * The server is authenticated to the client (through server certificate, no proxy server to redirect webpage)
      * HTTPS doesn't authenticate clients, it's computationally expensive and need more handshakes between the client and the server

##OO Design
* Cores
  * Abstraction
  * Inheritance
  * Polymorphism
  * Encapsulation
* UML legend
* Use MVC framework to help quickly setup elementary classes
* Note the usage of enum (how to initialize the enum with initial value)
  <pre><code>public enum Currency {
        PENNY(1), NICKLE(5), DIME(10), QUARTER(25);
        private int value;

        private Currency(int value) {
                this.value = value;
        }
  };
  </pre></code>   

* Dependency Injection
  * testing technique
  * Basically means giving an obejct its instance variables (dependency), instead of letting the object itself construct those instance variables
  * See comparison below
  <pre><code>// originally
    public someClass(){
      myObject = Factory.getObject();
    }
    public someClass(MyClass myObject){
      this.myObject = myObejct;
    }
  </pre></code>


##OO Design Patterns
[oodesign.com](http://www.oodesign.com/)<br>
[SO-Examples of GoF Design Patterns](http://stackoverflow.com/questions/1673841/examples-of-gof-design-patterns/2707195#2707195) <br>
[notes](https://docs.google.com/document/d/1tWOAhdr4QG_Y4GTpKjmx-OP6HnKFtnGp0I5KZi12LV0/edit?usp=sharing)
* Creational Patterns
  * Singleton:  java.lang.Runtime
      * how to implement? (private constructor)
      * how to ensure multithread safe? (enum, synchronized)
      * <pre><code>public class Singleton{
        private static Singleton sRef;
        private Singelton(){

        }
        public static Singelton getInstance(){
          if (sRef==null){
            synchronized(this){
              if (sRef==null)
                sRef = new Singleton();
            }
          }
          return sRef;
        }
      }</pre></code>
  * Abstract Factory / Factory Method
      * Abstract Factory returning the factory itself which in turn can be used to create another abstract/interface type
      * Factory Method returning an implementation of an abstract/interface type
  * Prototype
      * clone() -- deep/shalow clone
  * Builder: java.lang.StringBuilder#append()
      * returning the instance itself
      * implementations of java.lang.Appendable are good examples
      * Builder Vs Factory Method
* Structural Patterns
  * Adapter: java.util.Arrays#asList()
      * In real world we have adapters for power supplies, adapters for camera memory cards, and so on.
  * Decorator:
      * taking an instance of same abstract/interface type which adds additional behaviour
      * All subclasses of java.io.InputStream, OutputStream, Reader and Writer have a constructor taking an instance of same type.
      * java.util.Collections, the checkedXXX(), synchronizedXXX() and unmodifiableXXX() methods
  * Composite
      * There are times when a program needs to manipulate a tree data structure and it is necessary to treat both Branches as well as Leaf Nodes uniformly
      * File System -- folder and file
  * Flyweight
      * returning a cached instance, a bit the "multiton" idea
      * java.lang.Integer#valueOf(int) (also on Boolean, Byte, Character, Short and Long)
      * that's why *Integer.valueOf(int)* is preferred over *new Integer(int)*
  * Facade
      *  internally uses instances of different independent abstract/interface types
      *  wrap a poorly designed collection of APIs with a single well-designed API
      *  make a software library easier to use, understand and test, since the facade has convenient methods for common tasks
      *  [good example from wiki](http://en.wikipedia.org/wiki/Facade_pattern)
* Behavioral Patterns
  * Iterator: All implementations of java.util.Iterator 
      * One of the most common data structures in software development is what is generic called a collection. A collection should provide a way to access its elements without exposing its internal structure
      * An iterator that allows insertion and deletions without affecting the traversal and without making a copy of the aggregate is called a **robust iterator**.
      * **Multithreading Iterator**: all multithreading iterators should be robust iterators.
  * Observer: All implementations of java.util.EventListener
      * invokes a method on an instance of another abstract/interface type, depending on own state
  * Command: All implementations of java.lang.Runnable
      * the Macro represents, at some extent, a command that is built from the reunion of a set of other commands, in a given order. Just as a macro, the Command design pattern encapsulates commands (method calls) in objects 
      * the main advantage of the command design pattern is that it decouples the object that invokes the operation from the one that know how to perform it.
  * Strategy: 
      * java.util.Comparator#compare(), executed by among others Collections#sort().
      * There are common situations when classes differ only in their behavior. For this cases is a good idea to isolate the algorithms in separate classes in order to have the ability to select different algorithms at runtime.
      * the main draw back is that a client must understand how the Strategies differ. Since clients get exposed to implementation issues the strategy design pattern should be used only when the variation in behavior is relevant to them.
  * Chain of Responsibility
      * consisting of a source of command objects and a series of processing objects. Each processing object contains logic that defines the types of command objects that it can handle; the rest are passed to the next processing object in the chain

##Analyze Running Time -- (from CLRS)
* Analyze in a RAM model, which means that instructions are executed one after another, with no concurrent operations.
* Running time: the numebr of primitive operations or "stpes" executed.
* Worst case, best case and average case. Usually, we use worst case; and sometimes, average case is used (provided that all inputs of a given size are equally likely, we can use a randomized algorithm to achieve this)
* Order of Growth -- rate of growth of the running time
  * We're only concerned about the highest-order term (ignore the lower-order terms), and ignote constant coefficient.
  * e.g. in an^2 + bn + c, we only care about n^2
* Master Theorem
  * calculate the running time of recurrence equation
* big O, upper bound; omega, lower bound
  * The definitions of O-notation and o-notation are similar. The main difference
is that in f(n) = O(g(n)), the bound 0 <= f(n) <= cg(n) holds for some constant c > 0, but in f(n) = O(g(n)), the bound 0 <= f(n) < cg(n) holds for all constants c > 0
  * O(n!) > O(2^n) > O(n^2) > O(n) > O(lgn)
* Solving Recurrences
  * Substitution method (Guess)
    * T(n) = T(n-1) + 1/n, O(n) = lgn
    * T(n) = T(n-1) + lgn, O(n) = nlgn
    * T(n) = T(n-1) + n, O(n) = n^2
  * The recursion-tree method 
  * The master method, works for the recurrences of the form: T(n) = aT(n/b) + f(n)
    * compare f(n) with n^(lgba)
    * polynomially smaller, a is smaller than b by a factor (which is b/a) of n^c for some constant c > 0. e.g. a=n, b=nlgn, b/a=lgn, which is not polynomially small.

##Data Structure -- (from CLRS)
* Dynamic Set
  * Dictionary: a dynamic set that supports insertion, deletion and lookup (test membership in a set)
  * Ordering Set: supports operations like getMin(), getMax(), getPrev(), getNext()
* Stack
  * implementation
    * Array, push(), pop(), peek(), empty(), average O(1) if don't consider overflow
    * LinkedList, push(), pop(), peek(), empty(), all O(1)
* Queue
  * implementation
    * Array, one circular array, two pointers (head/tail)
    * LinkedList, two pointers (head/tail)
* Double-ended Queue
  * deque (pronounced deck), an abstract data type that generalizes a queue
  * elements can be added or removed from either the head or tail
  * implementation
    * dynamic array
    * doubly linked list

* Heap (Priority Queue)
  * parent node is always 'larger'(depends on comparator) than the child nodes 
  * getMax(), O(1); insert(), O(lgn); removeMax(), O(lgn);

* LinkedList
  * when cascade multiple pointers, rememebr to check null. e.g. node.next.next, should check if (node.next==null) in advance
  * Sentinel, a dummy object that allows us to simplify boundary conditions
    * make the code clean, should be used judiciously when the list is small (memory cost)
  * [XOR LinkedList](http://en.wikipedia.org/wiki/XOR_linked_list)
  * Doubly Linked List: two pointers, prev and next, easy for reverse traversal. The head and tail node are typically sentinel node or null to facilitate traversal of the list.
  * Circular Doubly Linked List: the head and tail are connected to form a circle.
* Binary Search Tree 
  * Query operations: search(), getMin(), getMax(), getPredecessor(), getSuccessor(), worst case O(h) (h is the height of the tree). In a balanced bst, worst case is O(lgn) (n is the number of nodes)
  * Modification operation
    * insertion, O(lgn), pretty trivial
    * deletion, O(lgn), 3 cases
      * has no child
      * has one child
      * has two children
* [Red Black Tree](http://www.geeksforgeeks.org/red-black-tree-set-1-introduction-2/)
  * a self-balancing binary search tree with one extra bit of storage per node: color (either RED or BLACK)
* [AVL tree](http://www.geeksforgeeks.org/avl-tree-set-1-insertion/)
  * a self-balancing binary search tree
  * the heights of the two child subtrees of any node differ by at most one; if at any time they differ by more than one, rebalancing is done by **tree rotations**(constant time)
  * running time of all basic bst operations are O(lgn)
  * The AVL trees are more balanced compared to Red Black Trees, but they may cause more rotations during insertion and deletion. So if your application involves many frequent insertions and deletions, then Red Black trees should be preferred. And if the insertions and deletions are less frequent and search is more frequent operation, then AVL tree should be preferred over Red Black Tree.
* Trie
  * a prefix tree, keys are usually strings
  * lookup, insertion, deletion, all O(m) m is the length of the key
  * Usage: e.g. Longest Prefix Matching, AutoComplete, SpellChecking
  * Advantages over balanced BST:
    * Lookup key is faster: 
      * trie, worst case O(m), m is the length of the key
      * balanced BST, worst case O(mlgn) (compare each key until end), n is the number of nodes in the tree
    * Space efficient
  * Advantages over hashmap
    * support ordered iteration, getSuccessor and getPredecessor
    * Hash tables are commonly said to have expected O(1) insertion and deletion times, but this is only true when considering computation of the hash of the key to be a constant time operation. When hashing the key is taken into account, hash tables have expected O(k) insertion and deletion times, but may take longer in the worst-case depending on how collisions are handled. Trie have worst-case O(k) insertion and deletion
  * Implementation
  <pre><code>TrieNode{
    char value;
    boolean isEnd;
    Map<Character, TrieNode> children;
  }
  </pre></code>
* [Radix Tree](http://en.wikipedia.org/wiki/Radix_tree)
  * a space optimized trie, where each node with only one child is merged with its child
  * Efficient for small sets (especially if the strings are long) and for sets of strings that share long prefixes
* Suffix Tree
  * Compressed trie of all suffix substrings
  * Usage: 
    * Longest Common Substring
    * Longest repeated substring
    * Longest palindrome in a string
    * Search string pattern
* [Segment Tree](http://www.geeksforgeeks.org/segment-tree-set-1-sum-of-given-range/)
  * a tree data structure (balanced binary tree) for storing intervals, or segments.
  * Provide efficient query for range values
  * Construction: O(n), total 2*n-1 nodes; Lookup, O(lgn); Update, O(lgn). (n is the number of elements in given array, also the number of leaf nodes)
  * One usage is in search engine. (e.g. [Lucene](http://blog.mikemccandless.com/2013/12/fast-range-faceting-using-segment-trees.html))
* [Ternary Search Tree](http://en.wikipedia.org/wiki/Ternary_search_tree)
  * A type of Trie.
  * Space efficient compared to standard prefix trees, at the cost of speed

* LinkedHashMap
  * Hash table and linked list implementation of the Map interface, with predictable iteration order, and without incurring the increased cost associate with TreeMap.
  * Maintains a doubly-linked list running through all of its entries. Thus, it defines the iteration ordering. There're two kinds of orders: access order, insertion order (which cen be decided in constructor)
  * it provides constant-time performance for the basic operations (add, contains and remove), assuming the hash function disperses elements properly among the buckets. Performance is likely to be just slightly below that of HashMap, due to the added expense of maintaining the linked list, with one exception: Iteration over the collection-views of a LinkedHashMap requires time proportional to the size of the map, regardless of its capacity. Iteration over a HashMap is likely to be more expensive, requiring time proportional to its capacity.

* Graph -- (from CLRS)
  * G = (V, E), V for vertex, E for edge
  * Representation of graph:
    * Object and pointer
    * Adjacency list, for sparse graph |E| < |V|^2
    * Adjacency matrices
  * Graph Searching Algorithms
    * DFS, BFS
    * Shortest path in unweighted undirected graph
      * BFS
    * Shortest path in weighted directed path (edge weight may be negative)
      * Bellman-Ford, O(VE)
        * return FALSE if there's negative cycle
    * Shortest path in weighted directed path (edge weight is non-negative)
      * Dijkstra, O(E + VlgV)
    * Shortest path in DAG
      * Topological sort, O(V+E)
    * Topological Sort in DAG (Direct Acyclic Graph)
      * DFS
    * Minimum Spinning Tree 
      * Kruskal, is a greedy algorithm, add edge of least weight incrementally, and UNION the disjoint sets if possible
      * Prim, similar to Dijkstra, is a greedy algorithm, add adjacent edge of least weight to the already-connect tree
      * An application is Travelling Salesman Problem

## NP (Nondeterministic-Polynomial)
  * P : problems can be solved in polymonial time
  * NP: problems don't necessarily run in a polymonial time on a regular computer, but do run in polymonial time on a nondeterministic Turing machine. (P is included in NP)
  * NP-hard: problems can be reduced from any problem in NP
  * NP-complete: both in NP-hard and NP
  * Decision problem & Optimization problem
    * eg, path of cost k exist? find the shortest path
  * Classic questions:
    * Travelling Salesman Problem

## Sorting
  * Comparison sort: requires O(nlgn) worst case
    * merge sort, quick sort, insertion sort, selection sort/bubble sort
  * Counting sort

## Android
[tests for Android knowledge base](http://skillgun.com/Questions.aspx?p_id=0&Qbc_id=17&Topicname=Android&typeid=0&count=0&QbMT_id=17&usrSubmit_qtn=0&chptrname=All&FAQ_Type=2)
* APK (Application Package File)
  * the package file format used to distribute and install application software and middleware onto Google's Android operating system
  * An APK file contains all of that program's code (such as .dex files), resources, assets, certificates, and manifest file

* Low Level Architecture
  * Android is an operating system based on Linux kernel, uses DVM (Dalvik Virtual Machine). DVM sits on library layer
    * .java -> .class -> .dex
  * Android team preferred DVM over JVM because of below given reasons.
    * 1. Though JVM is free it was under GPU license, which is not good for Android as most of the Android is under Apache license. 
    * 2. JVM was designed by keep desktops in mind. So it is too heavy for embedded devices.
    * 3. DVM takes less memory, runs & loads faster compared to JVM.
    * 4. Since mobile devices have lots of limitations like low CPU speed and less Memory, it is always better not to use heavy components like JVM.
  * Top two layers (application, and Framework layers) are written in Java, whereas drivers layer and library layer are written in C and C++.

* Support different devices (use resources in res/)
  * Language
    create additional values directories inside res/ that include a hyphen and the ISO country code at the end of the directory name. For example, **values-es/** is the directory containing simple resourcess for the Locales with the language code "es"
  * Screen
    create a unique layout XML file for each screen size you want to support. Each layout should be saved into the appropriate resources directory, named with a -<screen_size> suffix. For example, a unique layout for large screens should be saved under **res/layout-large/**
  * Platform version
    Specify minimum and target API in Manifest

* Activity
  *  a single screen in an application, each activity has a corresponding <activity> declaration in manifest file.
  * Lifecycle: onCreate -> onStart -> onResume -> onPause -> onStop -> onDestroy
    * onPause: the activity is partially obscured by another activity. eg, pops up one dialog. Save data into locale storage or database in onPause()
    * onStop: the activity is completely hidden and not visible to the user
  * Decalre a launcher activity in manifest 

* Fragment
  * Introduced in Android 3.0 (API 11)
  * Decompose application functionality and UI into reusable modules
  * Create a fragment
    * static, declare <fragment> in res/layout
    * dynamic, use FragmentManager
  * Communication with activity
    * frag -> act : define interface in frag, and implement interface in activity
    * act -> frag: fragment.setArguments(Bundle)
  * Lifecycle:
    * create: onAttach -> onCreate -> onCreateView -> onActivityCreated
    * start: onStart
    * resume: onResume
    * pause: onPause
    * destroy: onDestroyView -> onDestroy -> onDetach

* ActionBar
  * first introduced with API level 11 (Android 3.0) 
  * can use action bar on devices running Android 2.1 or higher by importing supported library

* Intent
  * An Intent is a messaging object you can use to request an action from another app component
  * 3 basic use-cases: start an activity, start a service, deliver a broadcast
  * implicit and explicit intent
  * put data into intent
  * get result from intent

* Manifest
  * Every application must have an AndroidManifest.xml file (with precisely that name) in its root directory. The manifest file presents essential information about your app to the Android system, information the system must have before it can run any of the app's code
  * It names the Java package for the application. The package name serves as a unique identifier for the application.
  * It describes the components of the application — the activities, services, broadcast receivers, and content providers
  * It declares processes
  * It declares permissions
  * It declares API level

* Permission
  * Declare
    * <uses-permission> in Manifest
  * Usage:
    * To restrict access to costly operations.
    * To restrict access to device hardware features.
    * To restrict access to user data.

* Service
  * A Service is an application component that can perform long-running operations in the background and does not provide a user interface
  * Not a process, not a thread. It runs in the main thread of the application that hosts it, by default
  * A service can allow other components to bind to it, in order to interact with it and perform interprocess communication

* Content Provider
  * Content providers manage access to a structured set of data. 
  * You don't need to develop your own provider if you don't intend to share your data with other applications
  * eg, Calendar Provider, Contacts Provider

* Configuration change
  * Some device configurations can change during runtime (such as screen orientation, keyboard availability, and language)
  * Android restarts the running Activity (onDestroy()is called, followed by onCreate())
  * The restart behavior is designed to help your application adapt to new configurations by automatically reloading your application with alternative resources that match the new device configuration

* Broadcast Receiver
  * A broadcast is a message that any app can receive
  * You can deliver a broadcast to other apps by passing an Intent to sendBroadcast(),

* ANR: Application Not Responding
  * Application has to respond to main UI thread in 5 seconds

* Context
  * allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activities, broadcasting and receiving intents
  * two kinds of Context, Activity and Application
  * to avoid context-related memory leaks, remember the following:
    * Do not keep long-lived references to a context-activity (a reference to an activity should have the same life cycle as the activity itself, otherwise, the reference will prevent GC from collecting the activity)
    * Try using the context-application instead of a context-activity







