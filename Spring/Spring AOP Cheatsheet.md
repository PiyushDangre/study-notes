# Spring AOP Cheatsheet & Notes

## Sources
- [Intro to AOP](https://www.digitalocean.com/community/tutorials/spring-aop-example-tutorial-aspect-advice-pointcut-joinpoint-annotations)
- [Pointcuts](https://www.baeldung.com/spring-aop-pointcut-tutorial)

## Introduction

- AOP stands for **Aspect Oriented Programming**.
- Spring AOP is the mechanism to separate out all the **cross cutting concerns** across the application (Such as logging, transaction management etc).
- What are **Cross Cutting Concerns** : They are common functionalities across the application which we would like to aggregate at one place.  For eg. we can write a separate aspect which has advices for logging functionality to be used for a set of methods.
- We use @Annotations for marking Spring AOP components. All of these annotations are present in `org.aspectj.lang.annotation` package.
- It has the following components :

    1. Aspect
    2. Advices
    3. Pointcut
    4. JoinPoint
    5. Target Object
    6. AOP Proxy
    7. Weaving
    
## Code changes to get started

In order to add Spring-AOP support in Spring Boot project, add the following dependency in POM.xml - 

```XML
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-aop</artifactId>
		</dependency> 
```
After this we are free to use Spring AOP annotations such as `@Aspect` etc.
## Components w/ code examples
### Aspect

- It is annotated by **@Aspect**
- This is the class in which we put all the advice methods. 
- That means Aspect class encapsulates all the Advices that we write in Sping AOP.
- There can be multiple classes which we can annotate with @Aspect.
- This class will have all the Advices which are annotated with appropriate @Before/@After annotations.
- In Spring Boot we have to add @Component after the @Aspect as the bean of the Aspect should be present in context. Only then it will work.
```Java
@Aspect
@Component
public class ApplicationAspects {

}
```

### JoinPoint
- It is a specific point in the application which shares some cross cutting concern across the application.
- The JoinPoint can be a method being called, exception being thrown  etc .
In Spring-AOP, only **method** joinpoint is allowed.
.
- We can make use of @Before/@After annotations to define a JoinPoint where the advice function needs to be called.
- There is no annotation for Joinpoint.
- So basically, joinpoint is the method before/after which we want our advice to run. 

### Advices
- Advice is the method which runs when a certain Pointcut matches a JoinPoint.
- Basically, it is the action that we want to be taken for a particular JoinPoint.
- Types of Advice annotations (or Advices) :-

    | Advice | Description |
    |---------|-----------------------------|
    | @Before  | Called before the joinpoint  |
    | @After  | Called after the joinpoint  |
    | @AfterReturning  | Called after the joinpoint method executes normally. This runs before @After.|
    | @AfterThrowing  | Called after the joinpoint method throws exception.  |
    | @Around  | Used to wrap logic before and after the joinpoint method.  |
---
###
Let's say we want some logic to be run before the `Employee.earnSalary()` method call as shown in below example - 

Employee.java
```java
@Component
public class Employee {
	
	public void earnSalary() {
		System.out.println("Employee earning Salary!");
	}

}
```

ApplicationAspects.java 
```java
@Aspect
@Component
public class ApplicationAspects {

	@Before("execution(public void earnSalary())") // Here the string passed to @Before() is called Pointcut Expression.
	public void runThisCodeBeforeEarningSalary() {
		System.out.println("Work for long hours!");
	}	
}
```

SpringAopApplication.java
```java
@SpringBootApplication
public class SpringAopApplication {

	public static void main(String[] args) {
		ApplicationContext ac = SpringApplication.run(SpringAopApplication.class, args);
		Employee emp = ac.getBean(Employee.class);
		emp.earnSalary();
	}
}
```

Final Result  in Terminal - 
```
Work for long hours!
Employee earning Salary!
```

### Pointcut

- Point cut are the expressions used to select the joinpoint.
- There can be various designations which can be used in PointCut.
1. **execution** : To select a method of any return type, anywhere in application code.
2. **within** : To select methods within a particuar class or package using fully qualified name.

**Various kinds of Pointcut expressions**

```java
	/**
	 * 1st * -> Any return type
	 * 2nd * -> Any method name after 'conca'
	 * (..) -> Any number of arguments of any type
	 */
	@After("execution(* com.dangre.springaop.models.Employee.conca*(..))")
	public void demo3() {
		System.out.println("Pointcut - Demo3");
	}
	
	/**
	 * 1st * -> Any return type
	 * 2nd * -> Any class fully qualified name
	 * 3rd * -> Any method name after 'conca'
	 * (..) -> Any number of arguments of any type
	 */
	@After("execution(* *.conca*(..))")
	public void demo4() {
		System.out.println("Pointcut - Demo4");
	}
	
	/**
	 * - Here we are using within
	 * - We can specify classname or package name here.
	 */
	@After("within(com.dangre.springaop.models.Employer)")
	public void demo5() {
		System.out.println("Pointcut - Demo5");
	}

```
### Using Around advice to manipulate input parameters

Let's say we have to write advice for this method -

```java
	public Integer getLeavesApplied(Period p) {
		System.out.println("Input parameter Period got in getLeavesApplied -> "+p);
		return 5;
	}
```

then in our **@Aspect** class we define the **@Around** advice -

```java
	@Around("execution(public Integer getLeavesApplied(*))") // * is used to make the Argument Object Generic
	public void runThisCodeAroundGetLeavesApplied(ProceedingJoinPoint pjp) throws Throwable {
		Object[] arg = new Object[1];
		arg[0]=Period.of(2, 0, 0);
		// Or we can create Object[] like this too -> Object[] arg = {Period.of(8, 0, 0)};
		Integer leavesNo = (Integer)pjp.proceed(arg); // Manipulating the original arguments passed to method call
		System.out.println("After leaves applied - "+leavesNo);
	}
```

We use ProceedingJoinPoint object as a parameter in above advice object. This is used to call the actual JoinPoint object. It returns the actual return value of the joinpoint. 

Output in Console ;-

```
Input parameter Period got in getLeavesApplied -> P2Y
After leaves applied - 5
```

### Creating a custom annotation using @Around
Our aim is to create a annotation which measures time taken by a method for execution.

First we declare the custom annotation we are gonna use that is @MonitorMethodTime

```java
/**
 * - We use @interface annotation to create our custom annotation.
 * - This is just a marker interface 
 */
public @interface MonitorMethodTime {

}
```

Then in our @Aspect class we declare the @Around advice - 

```java
	/**
	 * - We wrote a custom annotation @MonitorMethodTime
	 * - This annotation helps log the Method Start and Method End Time
	 * - It also logs the total time taken by a method after it finishes returning value.
	 * - We mark the method with @MonitorMethodTime and use the below advice to calculate the value of time taken for execution.
	 * 
	 * @throws Throwable 
	 */
	@Around("@annotation(com.dangre.springaop.models.MonitorMethodTime)")
	public void calculateTimeTakenByMethod(ProceedingJoinPoint pjp) throws Throwable {
		long beforeTime = System.currentTimeMillis();
		pjp.proceed();
		long afterTime = System.currentTimeMillis();
		long totalTimeTaken = afterTime - beforeTime;
		System.out.println("Total time taken in ms => "+totalTimeTaken+" by methodName: "+pjp.getSignature().getName());
	}
```
Then, finally we annotate the method for which we measure the performance -  

```java
	/**
	 * - We are using our custom annotation @MonitorMethodTime
	 * - By virtue of this annotation, we will log the total time taken by the method to execute after it completes execution.
	 * @return
	 * @throws InterruptedException
	 */
	@MonitorMethodTime
	public String reallySlowRunningMethod() throws InterruptedException {
		Thread.sleep(300);
		return "Method Completed!";
	}
```

Then we call the annotated method from main class :-

```java
emp.reallySlowRunningMethod();
```

Output in terminal - 

```
Total time taken in ms => 315 by methodName: reallySlowRunningMethod
```

In above example, we got method signature from `ProceedingJoinPoint` -> pjp.getSignature().getMethod()


<br>
<br>

---

