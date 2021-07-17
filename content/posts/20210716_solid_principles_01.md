---
title: "SOLID principles: Single Responsibility"
date: 2021-07-16T21:37:08+01:00
draft: false
tags: ["goals","solid","software"]
---

I will try to explain a bit the SOLID principles, that should be engraved in every software engineer mind!

This one is about the **Single Responsibility Principle**, yes it is the S in SOLID! (shocking, I know!)

This principle states that:
> Every software component should have one and only one responsibility

In order to better understand this principle, we need to talk about **cohesion** and **coupling**:
- **Cohesion** is the degree to which the various parts of a software component are related;
- **Coupling** is defined as the level of inter dependency between various software components.

### So, why and how should we care about cohesion?
We should care about having a high level of cohesion between all the pieces of the component we are designing. Imagine that you are creating a `Student` component that should handle the `Student` information, the classes to attend, the schedule, the grades, the holidays...

All of a sudden, we are seeing data that doesn't link between each other and you can imagine how confusing it can be for you to read a piece of code that handles things that are not related with each other.

When looking if the component follows the **Single Responsibility Principle** a good place to start is analyzing the cohesion levels between the different pieces of it.

### So, why and how should we care about coupling?
We should care about having the pieces of our component loosely coupled. This means that whenever something changes I don't have to go change all the other pieces of my component.

When looking if the component follows the **Single Responsibility Principle** check if the pieces depend on each other too much.

With this in mind, I guess there is an easier way for us to think about this principle, so let me rephrase it:
> Every software component should have one and only one reason to change

So, when reviewing your components, just ask yourself this question:

**How many reasons do I have to make a change to this component?**

Let's look at an example, so that it is easier to understand:

```python
class Student:
    def __init__(self, name: str, age: str):
        self.name = name
        self.age = age

    def set_name(self, name: str):
        self.name = name
    
    def get_name(self) -> str:
        return self.name

    def set_age(self, age: int):
        # validate age
        self.age = age
    
    def get_age(self) -> int:
        return self.age

    def save(self):
        # this would be the dependency to use
        # from mysql.connector import connect, Error
        try:
            with connect(
                host="localhost",
                user="username",
                password="password",
            ) as conn:
                create_db_query = "insert into student values (?,?)"
                with conn.cursor() as cursor:
                    cursor.execute(create_db_query, self.name, self.age)
        except Error as e:
            print(e)

    def calculate_tuition(self) -> float:
        if self.age <= 17:
            return 1000.0
        if self.age >= 30:
            return 1200.0
        return 1100.0
```

We have a `Student` class with two attributes `name` and `age` and several methods to interact with it.

Ok, let's start and analyze how many reasons we have to change the code:
1. Changes in the way we handle student information
3. Changes in the way we interact with the database
4. Changes in the way we calculate tuitions

There is more than one reason for us to change this class, so we can say that we are in violation of the **Single Responsibility Principle**.

If we look at our example, and check how our methods relate with each other:
- The `set_name`, `get_name` methods relate with each other since both deal with the name `Student`. Then, there is a high level of cohesion between both methods;
- The `set_age`, `get_age` methods relate with each other since both deal with the age `Student`. Then, there is a high level of cohesion between both methods;
- All those 4 methods are interacting with the `Student` properties, so there is a **high** level of cohesion between `get_age`, `set_age`, `get_name` and `set_name`;
- Although the `save` method reads the `Student` properties, it doesn't change them. So the cohesion level is **high** between the `save`, `get_age` and `get_name`, but **low** between `save`, `set_age` and `set_name`;
- The same applies to the `calculate_tuition` method.

So, let's try a different implementation:

```python
class Student:
    def __init__(self, name: str, age: str):
        self.name = name
        self.age = age

    def set_name(self, name: str):
        self.name = name
    
    def get_name(self) -> str:
        return self.name

    def set_age(self, age: int):
        # validate age
        self.age = age
    
    def get_age(self) -> int:
        return self.age

    def save(self):
        StudentRepository().save(self)

    def calculate_tuition(self):
        TuitionCalculator().calulate_tuition(self)

class StudentRepository:
    def save(self, student):
        # this would be the dependency to use
        # from mysql.connector import connect, Error
        try:
            with connect(
                host="localhost",
                user="username",
                password="password",
            ) as conn:
                create_db_query = "insert into student values (?,?)"
                with conn.cursor() as cursor:
                    cursor.execute(create_db_query, student.get_name, student.get_age)
        except Error as e:
            print(e)

class TuitionCalculator:
    def calculate_tuition(self, student) -> float:
        if student.age <= 17:
            return 1000.0
        if student.age >= 30:
            return 1200.0
        return 1100.0
```

Success! With this new implementation, each component has one and only reason to be changed, so we are following the **Single Responsibility Principle**.
