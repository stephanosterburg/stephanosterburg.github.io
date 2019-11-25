---
layout: post
title: 'Classes and Object Oriented Programming'
description: "Classes and Object Oriented Programming"
author: Stephan
categories: [maya, code, python]
tags: [maya, code, python]
featured_image_thumbnail: assets/images/posts/2019/python-object-oriented-programming.jpg
featured_image: assets/images/posts/2019/python-object-oriented-programming.jpg
featured: false
hidden: false
---

Python is an object-oriented language. It supports all the usual functionality of such languages such as classes, inheritance, and polymorphism. If you are not familiar with object-oriented programming (or scripting), it basically means it is easy to create modular, reusable bits of code, which are called objects. We have already been dealing with objects such as string objects and list objects. Below is an example of creating a string object and calling its methods.

```python
x = "happy"
x.capitalize()              # returns Happy
x.endswith("ppy")           # returns True
x.replace("py", "hazard")   # returns "haphazard"
x.find( "y" )               # returns 4
```

You are free to use Python without using any of its object oriented functionality by just sticking with functions and groups of statements as we have been doing throughout these notes. However, if you would like to create larger scale applications and systems, I recommend learning more about object oriented programming.

### Classes

Classes are the basic building blocks of object oriented programming. With classes, we can create independent instances of a common object. It is kind of like duplicating a cube a few times. They are all cubes, but they have their own independent attributes.

```python
class Shape(object):
    def __init__(self, name):
        self.name = name

    def print_me(self):
        print('I am a shape named {0}.'.format(self.name))

shape1 = shape(name='myFirstShape')
shape2 = shape(name='mySecondShape')

shape1.print_me()
I am a shape named myFirstshape.

shape2.print_me()
I am a shape named mySecondShape.
```

The above example shows a simple class that contains one data member (name), and two functions. Functions that begin with a double underscore usually have a special meaning in Python. The `__init__` function of a class is a special function called a constructor. It allows us to construct a new instance of an object. In the example, we create two independent instances of a shape object: shape1 and shape2. Each of these instances contains its own copy of the name attribute defined in the class definition. In the shape1 instance, the value of name is “myFirstShape”. In the shape2 instance, the value of name is “mySecondShape”. Notice we don’t pass in any value for the self argument. We don’t pass in any value for the self argument because the self argument refers to the particular instance of a class.

The first argument in all class member methods (functions) should be the self argument. The self argument is used to represent the current instance of that class. You can see in the above example when we call the print_me method of each instance, it prints the name stored in each separate instance. So objects are containers that hold their own copies of data defined in their class definition.

### Inheritance and Polymorphism

Inheritance and polymorphism are OOP constructs that let us build off of existing functionality. Say we wanted to add additional functionality to the previous shape class but we don’t want to change it because many other scripts reference that original class. We can create a new class that inherits the functionality of that class and then we can use that inherited class to build additional functionality:

```python
# Inherit from Shape
class PolyCube(Shape):
    def __init__(self, name, length, width, height):
        # Call the constructor of the inherited class
        super(PolyCube, self).__init__(name)

        # Store the data associated with this inherited class
        self.length = length
        self.width = width
        self.height = height

    def print_me(self):
        super(PolyCube, self).print_me()
        # The .2f in the string format means use 2 decimal places
        print('I am also a cube with dimensions {0:.2f}, {1:.2f}, {2:.2f}.'.format(
            self.length, self.width, self.height))


class PolySphere(Shape):
    def __init__(self, name, radius):
        # Call the constructor of the inherited class
        super(PolySphere, self).__init__(name)

        # Store the data associated with this inherited class
        self.radius = radius

    def print_me(self):
        super(PolySphere, self).print_me()
        print('I am also a sphere with a radius of {0:.2f}.'.format(self.radius))


cube1 = PolyCube('firstCube', 2.0, 1.0, 3.0)
cube2 = PolyCube('secondCube', 3.0, 3.0, 3.0)
sphere1 = PolySphere('firstSphere', 2.2)
sphere2 = PolySphere('secondSphere', 2.5)
shape1 = Shape('myShape')

cube1.print_me()
I am a shape named firstCube.
I am also a cube with dimensions 2.00, 1.00, 2.00.

cube2.print_me()
I am a shape named secondCube.
I am also a cube with dimensions 3.00, 3.00, 3.00.

sphere1.print_me()
I am a shape named firstSphere.
I am also a sphere with a radius of 2.20.

sphere2.print_me()
I am a shape named secondSphere.
I am also a sphere with a radius of 2.50.
```

In the above example, we create two new classes, PolyCube and PolySphere, that inherit from the base class, Shape. We tell a class to inherit from another class by placing the class to inherit from in parentheses when we declare the derived class. The two new classes will have all the data and methods associated with the shape base class. When we call the constructor method, __init__, of PolyCube and PolySphere, we still want to use the functionality of the constructor of its super class, shape. We can do this by calling the super class constructor explicitly with super(PolyCube, self).__init__(name). This will set the name variable since the Shape constructor initializes the name member variable. The second method, print_me, contains the added functionality of our new class. The method name is the same as the method name in the super class, Shape. However, when we call the method with cube1.print_me(), Python knows to use the method in PolyCube and not the method from Shape. This is called polymorphism. It allows us to replace, change, or add functionality to existing classes. By creating these hierarchies of objects, you can create pretty complex systems in neat, reusable classes that will keep your code clean and maintainable.

The previous example is an extremely simplified version of Maya’s architecture. Nodes inherit off of other nodes to build a complex hierarchy. Below is part of Maya’s object oriented node hierarchy:


{:refdef: style="text-align: center;"}
![Content by URL Count]({{ site.baseurl }}/assets/images/posts/2019/maya_node_hierarchy.png)
{: refdef}

---

<br>
