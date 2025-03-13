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
A for loop is used to iterate over a sequence (like a list, tuple, or string). It executes a block of code for each item in the sequence. For loop is pre
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
# [Functions](#functions)
Functions are reusable blocks of code that perform a specific task. Functions allow you to avoid repetition of codes and make your code more organized and readable. Functions allow you to break your program into smaller, manageable pieces, making it easier to read, maintain, and test. 
## [Defining and calling a function](#defining-and-calling-functions)
You define a function in Python using the `def` keyword.
```python
# Defining Functions

def greet(name):
    print(f"Hello, {name}!")

# calling the function
greet("Alice")
greet("Bob")
```
In this example, `greet` is the name of the function and `name` is the parameter. The function prints greeting text for the given parameter. The output of the above code will be:
`Hello, Alice!
Hello, Bob!
`

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