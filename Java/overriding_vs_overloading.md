### Rules of Method Overriding and Method Overloading

#### Method Overriding

- number of arguments can differ
- types of arguments can differ
- order of arguments can differ
- access modifiers can be different
- Return type has to be same (Otherwise compiler will complain)
- Overloaded methods cannot be static and non static (This will be method duplication - compiler will complain)

#### Method Overloading

- Method arguments have to be same. There should be no change in type or number of arguments.
- Return type should be same or subtype of original return type in superclass.
- Access modifier should be less restrictive than original. If original is public, subclass will have public - it cannot be private or protected. If original is protected, sublass can have protected or public. 
- Exception :
  - Overriding method can throw unchecked exception (any number) regardless of whether the exception is declared in original method.
  - Overriding method can throw checked exception which is narrower than the one declared in original method.
- Static method cannot be overridden and final method cannot be inherited (and overridden).
- Constructor cannot be overriden.

<img width="2155" height="1196" alt="Method_Inheritance_Rules" src="https://github.com/user-attachments/assets/0abb608e-f451-4e29-b6dc-1a25e73f38d6" />

  
