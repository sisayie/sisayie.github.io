---
title: Introduction to Programming with Python
subtitle: Beginner Tutorial
author: Dr. Sisay Adugna Chala
keywords: 
date: 2025-03-13
permalink: /posts/2025/03/introduction-python-programming/
tags:
  - python for absolute beginner
  - beginner
  - flow control
  - function
  - basic data structure
---

- [Introduction to Programming](#introduction)
- [Flow Control in Programming](#flow-of-control)
- [Functions](#functions)
- [Basic Data Structures](#basic-data-structures)
- [Practical Examples](#practical-examples)
- [Summary](#summary)
- [What is Next?](#next-steps)

# [Introduction to Programming](#introduction)

Programming is the process of giving a set of instructions to a computer to perform a specific task. Programming languages are used to write these instructions. Python is one of the most popular programming languages due to its: 

- Readable Syntax: Python’s syntax is simple and easy to understand, making it ideal for beginners.
- Versatility: Python is used in web development, data analysis, artificial intelligence, machine learning, automation, and more.
- Large Community: Python has a vast community, providing plenty of resources for learning.

# [Flow Control in Programming](#flow-of-control)
Flow control refers to the order in which individual statements, instructions, or function calls are executed in a program. There are three main types of flow control:
## [Linear Flow](#linear-flow)

Linear flow means the program runs from the top to the bottom, executing one instruction after another without any branching or looping as show in the example below:

``` python
# Linear Flow Example
print("Start of program")
x = 10
y = 5
sum = x + y
print("The sum is:", sum)
```
In this example, the program runs from top to bottom, executing one statement after another.

## [Conditional Statements (Making Decisions)](#conditions)
Conditional statements allow a program to make decisions based on whether a condition is true or false. The most common conditional statements are if, elif, and else.
### [if Statement](#if)
The if statement evaluates a condition and executes a block of code if the condition is true.
```python

age = 20

if age >= 18:
    print("You are an adult.")
```
### [else Statement](#else)
The else statement executes a block of code if the condition in the if statement is false.
```python

age = 16

if age >= 18:
    print("You are an adult.")
else:
    print("You are a minor.")
```
### [elif Statement](#elif)
The elif (i.e., short form of else if) statement allows you to check multiple conditions.
```python

age = 25

if age < 18:
    print("You are a minor.")
elif age < 21:
    print("You are an adult, but not yet 21.")
else:
    print("You are 21 or older.")
```
## [Looping (Iteration)](#looping)
Loops allow us to repeat a block of code multiple times. There are two common types of loops: for loops and while loops.
### [for Loop](#for-loop)
A for loop is used to iterate over a sequence (like a list, tuple, or string). It executes a block of code for each item in the sequence. For loop  is a powerful tool for automating repetitive tasks.
```python

# Iterating through a list using a for loop
fruits = ["apple", "banana", "cherry"]

for fruit in fruits:
    print(fruit)
```
In this example, `fruits` is the name of the variable containing the list above whereas `fruit` is the name of the variable that will hold the items in the list during the iteration.
### [while Loop](#while-loop)
A while loop repeatedly executes a block of code as long as a condition is True.
```python

# Using a while loop to print numbers from 1 to 5
count = 1
while count <= 5:
    print(count)
    count += 1  # Increment count by 1
```
### [When to use for Loop or while Loop?](#for-or-while)
Now that you understand for loop and while loop, you may have a question: "when is for loop preferred over while loop? when is while loop preferred over for loop?"

A for loop is typically preferred over a while loop when:

- Number of Iterations is Known or Finite:
  If you know in advance how many times you need to iterate, a for loop is often more appropriate. For example, iterating over a list or a fixed range of numbers as in the example below:
```python
for i in range(10):  # Iterates 10 times
    print(i)
```

- Iterating Over a Sequence or Collection:
  When you need to iterate over items in a sequence like a list, tuple, string, or dictionary, the for loop is cleaner and more readable.
```python
my_list = [1, 2, 3, 4]
for item in my_list:
    print(item)
```

- Clear Loop Boundaries:
  When the loop has well-defined start and end conditions, for loops help make the boundaries explicit, improving readability and reducing the chance of infinite loops.
```python
for i in range(0, 100, 10):  # Looping from 0 to 90 in steps of 10
    print(i)
```

- Iteration with Known Step Size:
  When you need to iterate through a range with a specific step size (e.g., incrementing or decrementing by a set number), a for loop is typically easier to use.
```python
for i in range(0, 20, 2):  # i goes 0, 2, 4, 6, ..., 18
    print(i)
```

If the number of iterations is unknown or dependent on a condition that might change during the loop (e.g., waiting for user input or processing data until a certain condition is met), a while loop is preferred. For example:
```python
# Continue until a valid user input is provided
while True:
    user_input = input("Enter a number: ")
    if user_input.isdigit():
        break
```

The `range()` function is commonly used to generate a sequence of numbers. It provides an easy way to iterate a specific number of times or over a defined sequence of values.

The basic syntax of `range()` is as follows:
```python
range(start, stop, step)
```
- **start**: The starting value of the sequence (inclusive). If not specified, it defaults to 0.
- **stop**: The ending value of the sequence (exclusive). This value is not included in the sequence.
- **step**: The difference between each number in the sequence. This is optional, and if not specified, it defaults to 1. Have a look at the following examples on the use of `range()` function:

1. **Basic range without start or step:**
   ```python
   for i in range(5):  # This will loop from 0 to 4 (not including 5)
       print(i)
   ```
   Output:
   ```
   0
   1
   2
   3
   4
   ```

2. **Range with start and stop:**
   ```python
   for i in range(2, 6):  # This will loop from 2 to 5 (not including 6)
       print(i)
   ```
   Output:
   ```
   2
   3
   4
   5
   ```

3. **Range with step:**
   ```python
   for i in range(1, 10, 2):  # This will loop from 1 to 9, with a step of 2
       print(i)
   ```
   Output:
   ```
   1
   3
   5
   7
   9
   ```

4. **Negative step:**
   ```python
   for i in range(10, 0, -2):  # This will loop from 10 to 2 (not including 0), stepping by -2
       print(i)
   ```
   Output:
   ```
   10
   8
   6
   4
   2
   ```

###### Key points about range() function:
- `start` is inclusive, but `stop` is exclusive. So, the loop will stop just before reaching the `stop` value.
- If `start` is greater than `stop` and the step is positive, the loop won't run. Similarly, if the step is negative and `start` is less than `stop`, the loop won't run.

### [break, continue and pass Statements](#break-continue-and-pass)
In python, there are three statements that are used to change the behavior of loops:
- break: Exits the loop prematurely. It is used to end the loop when some condition is satisfied. For example, if a loop is searching an item in a list, it should exit as soon as it gets the time.
- continue: Skips the rest of the current iteration and moves to the next one.
- pass is a statement that does nothing. It is often used as a placeholder or to intentionally ignore certain code blocks.
```python
# Using break and continue
for i in range(10):
    if i == 5:
        break  # Stop the loop when i equals 5
    if i % 2 == 0:
        continue  # Skip the rest of the loop if i is even
    print(i)
```

### Nested loops
There is also another variant of loop where you have loops inside loops (called nested loops). In Python, you can nest any type of loop (like a `for` loop or a `while` loop) inside another loop. Nested loops are commonly used when you need to perform more complex iterations, like working with 2D arrays, matrices, or iterating through multiple dimensions.

**Basic Structure**
```python
for outer_item in outer_sequence:
    for inner_item in inner_sequence:
        # Do something with outer_item and inner_item
```

- The **outer loop** runs first and controls how many times the **inner loop** will execute.
- The **inner loop** will complete all of its iterations for each single iteration of the outer loop. Have a look at the examples below:

**1: Nested `for` Loop with a List of Lists (2D list)**
Imagine you have a 2D list (a list of lists), and you want to loop through each row and each element in the row.

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for row in matrix:  # Outer loop (iterates over each row)
    for element in row:  # Inner loop (iterates over each element in the row)
        print(element, end=" ")
    print()  # Move to a new line after each row
```
Output:
```
1 2 3 
4 5 6 
7 8 9
```
As you can see from the output,
- the **outer loop** goes over each row of the matrix.
- the **inner loop** goes through each element in that row and prints it.
- the `end=" "` keeps the output on the same line, and `print()` without arguments moves to the next line after completing one row.

### Example 2: Nested Loops for Multiplication Table
A common example of a nested loop is generating a multiplication table. Here's how you can do it with two nested `for` loops:

```python
for i in range(1, 6):  # Outer loop for the first number (1-5)
    for j in range(1, 6):  # Inner loop for the second number (1-5)
        print(f"{i} x {j} = {i * j}", end="\t")
    print()  # Move to a new line after each row
```
Output:
```
1 x 1 = 1	1 x 2 = 2	1 x 3 = 3	1 x 4 = 4	1 x 5 = 5	
2 x 1 = 2	2 x 2 = 4	2 x 3 = 6	2 x 4 = 8	2 x 5 = 10	
3 x 1 = 3	3 x 2 = 6	3 x 3 = 9	3 x 4 = 12	3 x 5 = 15	
4 x 1 = 4	4 x 2 = 8	4 x 3 = 12	4 x 4 = 16	4 x 5 = 20	
5 x 1 = 5	5 x 2 = 10	5 x 3 = 15	5 x 4 = 20	5 x 5 = 25
```

### Example 3: Nested Loops with a Condition
You can also combine nested loops with conditions. For example, let's print the elements of a 2D list but only if they are even:

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for row in matrix:
    for element in row:
        if element % 2 == 0:  # Check if the element is even
            print(element, end=" ")
    print()  # Move to a new line after each row
```
Output:
```
2 
4 6 
8
```

### Example 4: Nested `while` Loop
You can also nest `while` loops. Here's an example of nested `while` loops that print a pattern:

```python
i = 1
while i <= 3:  # Outer while loop
    j = 1
    while j <= 3:  # Inner while loop
        print(f"i={i}, j={j}")
        j += 1
    i += 1
```
Output:
```
i=1, j=1
i=1, j=2
i=1, j=3
i=2, j=1
i=2, j=2
i=2, j=3
i=3, j=1
i=3, j=2
i=3, j=3
```

### Key Points:
1. **Outer loop** runs once for each item in its sequence, and for every iteration, the **inner loop** runs through its entire sequence.
2. **Performance considerations**: Nested loops can become slow if the sequences you're iterating over are large because the time complexity increases with the depth of nesting. For example, two nested loops over lists of length `n` result in a time complexity of `O(n^2)`.
3. **Indentation**: In Python, proper indentation is critical. The body of each loop must be indented.

### Conclusion:
Nested loops are a powerful tool when dealing with multi-dimensional data structures or when you need to perform repetitive tasks with multiple layers of iteration. Just remember that the deeper you nest loops, the more computationally expensive it becomes!

# [Functions](#functions)
Functions are reusable blocks of code that perform a specific task. Functions allow you to avoid repetition of codes and make your code more organized and readable. Functions allow you to break your program into smaller, manageable pieces, making it easier to read, maintain, and test.

## [Defining and calling a function](#defining-and-calling-functions)
You define a function in Python using the `def` keyword.
```python
# Defining a function

def greet(name):
    print(f"Hello, {name}!")

# calling the function
greet("Alice")
greet("Bob")
```

In this example, `greet` is the name of the function and `name` is the parameter. The function prints greeting text for the given parameter. The output of the above code will be:
```
Hello, Alice!
Hello, Bob!
```

## [Function with Return Value](#function-with-return-value)
Functions can return values using the return keyword. This allows you to use the result of the function in other parts of the program.
```python

def add(x, y):
    return x + y

result = add(5, 10)
print("Sum:", result)
```
In this example, the function `add` returns the sum of the numbers given to it as input and returns their sum.

# [Data Structures](#data-structures)
Data structures are containers used to store and organize data. Python has several built-in data structures:
## [Lists](#lists)
A list is an ordered collection of items. Lists are mutable, meaning you can change their content.
```python

fruits = ["apple", "banana", "cherry"]
fruits.append("orange")  # Add a new item to the list
print(fruits[0])  # Accessing the first item
```

## [Tuples](#tuples)
A tuple is similar to a list but immutable. Once created, you cannot change the values in a tuple.
```python

coordinates = (10, 20)
print(coordinates[0])  # Accessing the first value in the tuple
```

## [Dictionaries](#dictionaries)
A dictionary is a collection of key-value pairs. Each key is unique, and it maps to a corresponding value.
```python

person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

print(person["name"])  # Accessing a value by key
```
## [Sets](#sets)
A set is an unordered collection of unique items. Sets do not allow duplicates.
```python

fruits = {"apple", "banana", "cherry"}
fruits.add("orange")  # Adding an item to the set
print(fruits)
```
# [Summary](#summary)
In this tutorial, you have learned:
-	Basic programming concepts: What programming is, why Python is a great language to start with.
-	Flow control: Linear flow, decision making with if, elif, and else, and iterations using for and while loops.
-	Functions: How to define and use functions for reusable code blocks.
-	Data structures: Working with lists, tuples, dictionaries, and sets to store and organize data.

To revise the concepts, implement the following practical examples to help reinforce your learning:
Example 1: Simple Calculator
```python

def add(x, y):
    return x + y

def subtract(x, y):
    return x - y

def multiply(x, y):
    return x * y

def divide(x, y):
    return x / y if y != 0 else "Cannot divide by zero!"

# Test the calculator
print("Addition:", add(10, 5))
print("Subtraction:", subtract(10, 5))
print("Multiplication:", multiply(10, 5))
print("Division:", divide(10, 5))
```
Example 2: Odd/Even Checker
```python

def check_even_odd(number):
    if number % 2 == 0:
        return "Even"
    else:
        return "Odd"

num = 7
print(f"{num} is {check_even_odd(num)}.")
```

# [What is Next?](#next-steps)
Before deliving into advanced topics:
- Practice the examples and modify them to fit your needs.
- Solve problems on coding platforms like w3schools, LeetCode, Codewars, or HackerRank to improve your skills.

---
Now that you have read the article till the end, did you find this blog post useful? Drop me your comment as a message on [LinkedIn](www.linkedin.com/in/sisayie)