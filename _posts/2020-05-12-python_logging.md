---
layout: post
title: 'Logging in Python (Part 1)'
description: "Logging in Python (Part 1)"
author: Stephan
categories: [code, python]
tags: [code, python]
featured_image_thumbnail: assets/images/posts/2020/python_logging.png
featured_image: assets/images/posts/2020/python_logging.png
featured: False
hidden: False
---

If you are anywhere close to what I more or less used to do to debug my code using print statements all over the place to find where I made a mistake, then the following blog hopefully is helpful.

A few weeks back, I watched a python video on youtube and found myself wondering why the author uses `print(“[LOG] ...”)` and `print(“[ERROR] ...”)` to record what is happening in the code. Not that I already knew about the `logging` function in python, but I unconsciously kept ignoring it. To make it a habit, I decided to write a blog.

The `logging` module is part of the standard Python library and provides tracking for events that occur while the software runs. You can add logging calls to your code to indicate what events have happened.

The `logging` module allows for both diagnostic logging that records events related to an application’s operation, as well as audit logging, which records the events of a user’s transactions for analysis. It is primarily used to record events to a file.

### Table of Logging Levels

As a developer, you can ascribe a level of importance to the event that is captured in the logger by adding a severity level. The severity levels are shown in the table below.

Logging levels are technically integers (a constant), and they are all in increments of 10, starting with `NOTSET` which initializes the logger at the numeric value of 0.

You can also define your own levels relative to the predefined levels. If you define a level with the same numeric value, you will overwrite the name associated with that value.

The table below shows the various level names, their numeric value, what function you can use to call the level, and what that level is used for.

|   Level    | Numeric Value |      Function        |                               Used to                               |
|:----------:|:-------------:|:--------------------:|:-------------------------------------------------------------------:|
| `CRITICAL` |       50      | `logging.critical()` | Show a serious error, the program may be unable to continue running |
|   `ERROR`  |       40      |   `logging.error()`  |                     Show a more serious problem                     |
|  `WARNING` |       30      |  `logging.warning()` |       Indicate something unexpected happened, or could happen       |
|   `INFO`   |       20      |  `logging.info()`    |             Confirm that things are working as expected             |
|   `DEBUG`  |       10      |   `logging.debug()`  |             Diagnose problems, show detailed information            |

The `logging` module sets the default level at `WARNING`, so `WARNING`, `ERROR`, and `CRITICAL` will all be logged by default. In the example above, we modified the configuration to include the `DEBUG` level with the following code:

```python
logging.basicConfig(level=logging.DEBUG)
```

You can read more about the commands and working with the debugger from the official logging documentation.


### Why Use the `logging` Module

The `logging` module keeps a record of the events that occur within a program, making it possible to see output related to any of the events that occur throughout the runtime of a piece of software.

You may be more familiar with checking that events are occurring by using the `print()` statement throughout your code. The `print()` statement does provide an essential way to go about debugging your code to resolve issues. While embedding `print()` statements throughout your code can track the execution flow and the current state of your program, this solution proves to be less maintainable than using the logging module for a few reasons:
It becomes difficult to distinguish between debugging output and standard program output because the two are mixed.
When using `print()` statements dispersed throughout code, there is no easy way to disable the ones that provide debugging output.
It becomes difficult to remove all the `print()` statements when you are done with debugging.
No log record contains readily available diagnostic information.

It is a good idea to get in the habit of using the `logging` module in your code as this is more suitable for applications that grow beyond simple Python scripts and provides a sustainable approach to debugging.

Because logs can show you behavior and errors over time, they also can give you a better overall picture of what is going on in your application development process.

### Printing Debug Messages to Console

If you are used to using the `print()` statement to see what is occurring in a program, you may be used to seeing a program that defines a class and instantiates objects that looks something like this:

```python
class Pasta():
    def __init__(self, name, price):
        self.name = name
        self.price = price
        print(f"Pasta created: {self.name} (${self.price})")

    def make(self, quantity=1):
        print(f"Made {quantity} {self.name} pasta")

    def eat(self, quantity=1):
        print(f"Ate {quantity} pasta".)

pasta_01 = Pasta("Artichoke", 15)
pasta_01.make()
pasta_01.eat()

pasta_02 = Pasta("Margherita", 12)
pasta_02.make(2)
pasta_02.eat()
```

The code above has an `__init__` method to define the `name` and `price` of an object of the `Pasta` class. It then has two methods, one called `make()` for making pasta, and one called `eat()` for eating pizzas. These two methods take in the parameter of `quantity,` which is initialized with `1`.

Now let’s run our code we’ll receive the following output:

```bash
Output
Pasta created: artichoke ($15)
Made 1 Artichoke pasta
Ate 1 pasta(s)
Pasta created: Margherita ($12)
Made 2 Margherita pasta
Ate 1 pasta
```

While the `print()` statement allows us to see that the code is working, we can use the logging module to do this instead.

Let’s remove or comment out the `print()` statements throughout the code, and add import logging to the top of the file:

```python
import logging

class Pizza():
    def __init__(self, name, value):
        self.name = name
        self.value = value
...
```

The `logging` module has a default level of `WARNING,` which is a level above `DEBUG.` Since we’re going to use the `logging` module for debugging in this example, we need to modify the configuration so that the level of `logging.DEBUG` will return information to the console for us. We can do that by adding the following line below the import statement:

```python
import logging

logging.basicConfig(level=logging.DEBUG)


class Pasta():
...
```

This level of `logging.DEBUG` refers to a constant integer value that we reference in the code above to set a threshold. The level of `DEBUG` is 10.

Now, let us replace all of the `print()` statements with `logging.debug()` statements. Unlike `logging.DEBUG, ` which is a constant, `logging.debug()` is a method of the `logging` module. When working with this method, we can make use of the same string passed to `print(),` as shown below.

```python
import logging

logging.basicConfig(level=logging.DEBUG)


class Pasta():
    def __init__(self, name, price):
        self.name = name
        self.price = price
        logging.debug(f"Pasta created: {self.name} (${self.price})")

    def make(self, quantity=1):
        logging.debug(f"Made {quantity} {self.name} pasta")

    def eat(self, quantity=1):
        logging.debug(f"Ate {quantity} pasta")

pasta_01 = Pasta("Artichoke", 15)
pasta_01.make()
pasta_01.eat()

pasta_02 = Pasta("Margherita", 12)
pasta_02.make(2)
pasta_02.eat()
```

At this point, when we run the code we’ll receive this output:

```bash
Output
DEBUG:root:Pasta created: artichoke ($15)
DEBUG:root:Made 1 artichoke pasta
DEBUG:root:Ate 1 pasta
DEBUG:root:Pasta created: Margherita ($12)
DEBUG:root:Made 2 Margherita pasta
DEBUG:root:Ate 1 pasta
```

The log messages have the severity level `DEBUG` as well as the word `root` embedded in them, which refers to the level of your Python module. The `logging` module can be used with a hierarchy of loggers that have different names so that you can use a different logger for each of your modules.

For example, you can set loggers equal to different loggers that have different names and different output:

```python
logger1 = logging.getLogger("module_1")
logger2 = logging.getLogger("module_2")

logger1.debug("Module 1 debugger")
logger2.debug("Module 2 debugger")
```

```
Output
DEBUG:module_1:Module 1 debugger
DEBUG:module_2:Module 2 debugger
```

In the next blog, I will go over how to use the `logging` module to print messages out to a file.
