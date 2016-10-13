#TDD exercise, solution first steps
## String Calculator

### Build
Compile:
```sh
#!/bin/bash    
javac src/main/java/is/ru/stringcalculator/*.java -d classes
```    
Clean:
```sh     
#!/bin/bash
rm -r classes/*
```
Compile tests:
```sh
#!/bin/bash
javac -classpath "classes/:lib/junit-4.11.jar" src/test/java/is/ru/stringcalculator/*.java -d classes
```
Unit Test:
```sh
#!/bin/bash
java -cp "classes/:lib/junit-4.11.jar:lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore is.ru.stringcalculator.CalculatorTest
```

 Build and Test:
 ```sh
  #!/bin/bash
./bin/clean
./bin/compile
./bin/compile_tests
./bin/unit_test
```

### TDD

#### Test Empty
CalculatorTest
```java
@Test
public void testEmptyString() {
  assertEquals(0, Calculator.add(""));
}
```
Calculator    
```java
public static int add(String test){
  return 0;
}
```

#### Test One Number
CalculatorTest
```java
@Test
public void testOneNumber(){
  assertEquals(1, Calculator.add("1"));
}
```

Calculator
```java
public static int add(String text){
  if(text.equals("")){
    return 0;
  }
  else
  	return 1;
}
```        	
#### Test Two Number

CalculatorTest:
```java    
@Test
public void testTwoStrings(){
  assertEquals(3, Calculator.add("1,2"));
}
```
Calculator
```java
public static int add(String text){
  if(text.equals("")){
    return 0;
  }
  else if(text.contains(",")){
    String[] numbers = text.split(",");
    return Integer.parseInt(numbers[0]) + Integer.parseInt(numbers[1]);
  }
  else
	  return 1;
}
```
After Refactor
```java
public static int add(String text){
  if(text.equals("")){
    return 0;
  }
  else if(text.contains(",")){
    String[] numbers = text.split(",");
    return toInt(numbers[0]) + toInt(numbers[1]);
  }
  else
	  return 1;
}

private static int toInt(String number){
  return Integer.parseInt(number);
}

```

#### Test Multiple Numbers

CalculatorTest:
```java
@Test
public void testMultipleNumbers(){
	assertEquals(6, Calculator.add("1,2,3"));
}
```
Calculator:
```java
else if(text.contains(",")){
  String[] numbers = text.split(",");
  int total = 0;
  for(String number : numbers){
    total += toInt(number);
}
  return total;
}
```
After refactoring:
```java
public static int add(String text){
  if(text.equals("")){
    return 0;
  }
  else if(text.contains(",")){
    return sum(splitNumbers(text));
  }
  else
    return 1;
}

private static String[] splitNumbers(String numbers){
  return numbers.split(",");
}

private static int sum(String[] numbers){
  int total = 0;
  for(String number : numbers){
    total += toInt(number);
  }
  return total;
}
``` 
