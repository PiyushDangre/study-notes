### Singleton Class

- Private constructor
- Intitialization of data member of type of own class while declaration itself - or using static initialization block. So the class will contain a data member of its own type encapsulated (or declared) within itself. This will be declared as static. 
- Private constructor - and it will be blank. 
- Presence of static getInstance() method which returns the private data member of own class type. 

```JAVA
package com.tutorialspoint;

class Singleton {

   private static Singleton singleton = new Singleton( );

   /* A private Constructor prevents any other
    * class from instantiating.
    */
   private Singleton() { }

   /* Static 'instance' method */
   public static Singleton getInstance( ) {
      return singleton;
   }

   /* Other methods protected by singleton-ness */
   protected void demoMethod( ) {
      System.out.println("demoMethod for singleton");
   }
}
public class Tester {

   public static void main(String[] args) {
      Singleton tmp = Singleton.getInstance( );
      tmp.demoMethod( );
   }
}
```

- Lazy initialization of the class - so here the class is not initialized until getInstanmce() is called.
- Also notice the way below the singleton class has been made thread safe - by using synchronized keyword for getInstance(). 

```Java
package com.tutorialspoint;

class ClassicSingleton {

   private static ClassicSingleton instance = null;
   private ClassicSingleton() {
      // Exists only to defeat instantiation.
   }

   public static synchronized  ClassicSingleton getInstance() {
      if(instance == null) {
         instance = new ClassicSingleton();
      }
      return instance;
   }
   
   protected void demoMethod( ) {
      System.out.println("demoMethod for singleton");
   }
}
public class Tester {

   public static void main(String[] args) {
      ClassicSingleton tmp = ClassicSingleton.getInstance( );
      tmp.demoMethod( );
   }
}
```
