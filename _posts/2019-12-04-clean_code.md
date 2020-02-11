---
layout: post
title: 'Clean Code'
description: "Clean Code"
author: Stephan
categories: [code, python]
tags: [code, python]
featured_image_thumbnail: assets/images/posts/2019/clean-python-code.jpg
featured_image: assets/images/posts/2019/clean-python-code.jpg
featured: true
hidden: true
---

Writing clean, readable code is something we should all strive for. If you’re just learning to program, you may not be paying attention to how clean your code is, but after a while when you start working with other people with a shared code base you may begin to recognize the importance of writing clean code. Code is read a lot more than it is written and sloppy code just depresses and frustrates people. Being sloppy up front when you are under pressure actually slows you down in the long term. You will end up creating more bugs which leads to more maintenance.

In this post, I’ll give brief pointers on how to make your code cleaner and easier to read. For more in-depth discussions on writing clean code, I recommend the excellent book, `Clean Code: A Handbook of Agile Software Craftsmanship`, by Robert C Martin:


<!-- <iframe type="text/html" width="336" height="550" frameborder="0" allowfullscreen style="max-width:100%" src="https://read.amazon.com/kp/card?asin=B00DDZPC9S&preview=inline&linkCode=kpe&ref_=cm_sw_r_kb_dp_EOGZDb45SW762" ></iframe> -->

<iframe type="text/html" width="336" height="550" frameborder="0" allowfullscreen style="max-width:100%" src="https://read.amazon.com/kp/card?asin=B001GSTOAM&preview=inline&linkCode=kpe&ref_=cm_sw_r_kb_dp_8NIZDb9T07E1D" ></iframe>

---

### The DRY Principle

DRY stands for Don’t Repeat Yourself. You should state a piece of logic once and only once. If you find yourself copying and pasting chunks of code multiple times, that should be a signal that you are repeating yourself. Repeating code just leads to more code to maintain and debug. For example, the following code has repeated code:

```python
sphere = create_poly_sphere(name='left_eye')
assign_shader(sphere, 'blinn1')
parent_constrain(head, sphere)

sphere = create_poly_sphere(name='right_eye')
assign_shader(sphere, 'blinn2')
parent_constrain(head, sphere)
```

This code should be written like the following:

```python
def create_eye(name, shader):
    sphere = create_poly_sphere(name=name)
    assign_shader(sphere, shader)
    parent_constrain(head, sphere)

create_eye('left_eye', 'blinn1')
create_eye('right_eye', 'blinn2')
```

The second code example is easier to maintain. If an update needs to be implemented, we only have to update code in one place rather than multiple places.

### Use Clean Names

The names of your classes, variables, and functions contribute greatly to how readable and clean your code is. Take this code snippet from an actual VFX studio pipeline tool:

```python
curr = os.environ.get('CURRENT_CONTEXT')
if curr:
    cl = curr.split('/')

self.__curr = [None] * 6
self.setType( cl[0] )
self.setSeq( cl[1] )

if len( cl ) > 3:
    self.setSubseq( cl[2] )
    self.setShot( '/'.join( cl[2:] ) )
else:
    self.setShot( cl[-1] )

if wa: self.__wa = wa
else: self.__wa = os.environ.get('CURRENT_WORKAREA')
```

Can you tell what this code is doing? If you’re familiar with writing pipeline environment tools, you might recognize what it is trying to do. What is `self.__curr`? Why is it a list of 6 values? What do the elements of `cl` repreprent? Why does the length of cl being greater than 3 differentiate one block of the if statement from the other?

A cleaner implementation would look something like this:

```python
context_type, sequence, subsequence, shot = self.get_current_context()
self.set_type(context_type)
self.set_sequence(sequence)

if subsequence:
    self.set_subsequence(subsequence)
if shot:
    self.set_shot(shot)

self.__work_area = work_area if work_area else self.get_current_work_area()
```

The above code cleans up all the list indices and string manipulations to make the code easier to read and understand. The individual list elements have been assigned meaningful names. Also the environment variable accesses have been extracted away into new methods. This makes the code easier to maintain. What happens if we want to change the name of the environment variables or maybe we want to read the values from a configuration file on disk? Extracting those values to functions would let us update the code in one place rather than multiple direct accesses to the environment variable.

#### Naming Classes

Class names should be a noun because they represent objects. The name should be as specific as possible. If the name cannot be specific, it may be a sign that the class needs to be split into smaller classes. Classes should have a single responsibility.

| Bad class names include: | Good class names include: |
|--------------------------|---------------------------|
| ShapeIE                  | ShapeExporter             |
| Utility                  | RigPublisher              |
| Common                   | Project                   |
| MyFunctions              | User                      |
| DansUtils                |                           |
| ShapeClass               |                           |

#### Naming Methods

Method names should be verbs because they perform actions. There should be no need to read the contents of a method if the name accurately describes what the method does. If the function is doing one thing (as it should) it should be easy to name. If not, split the code into smaller functions. Sometimes explaining the code out loud and help you name the function. If you say “And”, “If”, or “Or” it is a warning sign that the method is doing too much.

| Bad method names include: | Good method names include: |
|---------------------------|----------------------------|
| proc_new                  | create_process             |
| pending                   | is_pending                 |
| process1                  | send_notification          |
| process2                  | import_mesh                |
|                           | calculate_rivet_matrix     |

Methods should only perform the actions described by the name. Any other actions are called side effects and they can confuse people using your code. For example, a method called validate_form should not save the form. A method called publish_model should not smooth the normals. A method called prune_weights should not remove unused influences.

#### Avoid Abbreviations

Abbreviated text may be easier to type, but code is read more than it is written. When people talk about code or read it silently, it is harder to say the abbreviations. There are also no standards when referring to abbreviations.

| Bad names: | Good names:  |
|------------|--------------|
| sjData     | subjob_data  |
| jid        | job_id       |
| sjid       | subjob_id    |
| nm         | name         |
| sjState    | subjob_state |

#### Naming Booleans

Boolean values should be able to fit in an actual sentence of saying something is True or False.

| Bad boolean names: | Good Boolean names: |
|--------------------|---------------------|
| open               | is_open             |
| status             | logged_in           |
| login              | is_valid            |
|                    | enabled             |
|                    | done                |

#### Symmetrical Names

When names have a corresponding opposite, be consistent and always use the same opposite.

| Bad naming  | Good naming |
|-------------|-------------|
| on/disabled | on/off      |
| quick/slow  | fast/slow   |
| lock/open   | lock/unlock |
| low/max     | min/max     |

### Working with Booleans

When comparing Booleans, compare them implicitly:

```python
# Don't do this
if (is_valid == True):
    # do something

# Instead do this
if is_valid:
    # do somethingCopy
```

When assigning booleans, assign them implicitly:

```python
# Don't do this
if len(items) == 0:
    remove_entry = True
else:
    remove_entry = False

# Instead do this
remove_entry = len(items) ==
```

Avoid using booleans that represent negative values. This leads to double negatives, which end up confusing people:

```python
# Don't do this
if not not_valid:
    pass

# Instead do this
if valid:
    pass
```
### Use Ternaries

Ternaries are ways of assigning a value to a variable depending on if some condition is True or False. For example:

```pytohn
# Don't do this
if height > height_threshold:
    category = 'giant'
else:
    category = 'hobbit'

# Instead do this
category = 'giant' if height > height_threshold else 'hobbit'
```

### Don’t Use String as Types

You may have encountered code similar to the following:

```python
if component_type == 'arm':
    # do something
elif component_type == 'leg':
    # do something else
```

This is considered bad form for various reasons. If we end up wanting to change the value of one of these types, we have to change it in all the places that it is used. It can also lead to typos and inconsistencies. A better approach would be:

```python
class ComponentType(object):
    arm = 'arm'
    leg = 'leg'

if component_type == ComponentType.arm:
    # do something
elif component_type == ComponentType.leg:
    # do something else
```

The new code provides one place to change and update values (the DRY principle). It also provides auto-completion support and is more searchable if you are using an IDE like PyCharm or Eclipse.

### Don’t Use Magic Numbers

Magic numbers are numeric values that seemed to have been pulled out of nowhere. For example, the following code was pulled from an actual VFX pipeline tool:

```python
if run_mode < 3:
    run_mode = 5
elif run_mode == 3:
    run_mode = 4
```

What do these numbers mean? You would need to search all over code that could span multiple files to figure out what these numbers represent. A better approach would be:

```python
class JobStatus(object):
    waiting = 1
    starting = 2
    running = 3
    aborting = 4
    done = 5

    def __init__(self, value=JobStatus.waiting):
        self.status = value

    def not_yet_running(self):
        return self.status < JobStatus.running

    def abort(self):
        if self.not_yet_running():
            self.status = JobStatus.done
        elif self.status == JobStatus.running:
            self.status = JobStatus.aborting

# job_status is the new run_mode
job_status.abort()
```

### Encapsulate Complex Conditionals

Sometimes you may have conditionals with many comparisons chained together. At some point, this is going to get hard to read. For example:

```python
# Instead of this
if (obj.component.partial_path.startswith('model') and
    namespace == ‘GEOM’ and
    has_rigging_publish(obj.child) and
    edits_path):

# Encapsulate the complex conditional in a function or variable
def is_model_only_publish(obj):
    return (obj.component.partial_path.startswith('model') and
            namespace == 'GEOM' and
            has_rigging_publish(obj.child) and
            edits_path)

if is_model_only_publish(obj):
```

### Writing Clean Functions

Functions should be created in order to help convey intent. They should do one thing and one thing only as this aids the reader, promotes reuse, eases testing, and avoids side effects. Strive for a function to only have 0-3 parameters with a max of 7-9 parameters. Anything longer makes it harder for readers to keep track of all the parameters while running through the code in their head. Functions should be relatively short, maybe no more than 100 or so lines. If a function is longer, it may be time to refactor (update) the code into smaller functions.

#### Extracting a Method

If you find your code 3 or 4 indentation levels deep, it may be time to extract some of that code into a separate function. For example:

```python
# Instead of this
if something:
    if something_else:
        while some_condition:
            # do something complicated

# Do this instead
if something:
    if something_else:
        do_complicated_things()

def do_complicated_things():
    while some_condition:
        # do something complicated
```

#### Return Early

People can usually only keep track of a handful of trains of thought at a time. Therefore, we should try to organize our code in as many discrete independent chunks as possible. For example:

```python
# Instead of this
def validate_mesh(mesh):
    result = False
    if has_uniform_scale(mesh):
        if has_soft_normal(mesh):
            if name_is_alphanumeric(mesh):
                result = name_is_unique(mesh)
    return result

# Do this
def validate_mesh(mesh):
    if not has_uniform_scale(mesh):
        return False
    if not has_normal(mesh):
        return False
    if not name_is_alphanumeric(mesh):
        return False
    return name_is_unique(mesh)
```

This is not a strict rule. Like everything listed so far, use it when it enhances readability.

Signs Your Function is Too Long
Functions should hardly ever be over 100 lines. Longer functions are harder to test, debug, and maintain since users need to keep track if updates at the start of the function affect areas and the end of the function. Here are some simple rules to determine if a function is too long:

* You separate sections of code in a function with whitespace and/or comments
* Scrolling is required to view all the code.
* The function is hard to name.
* There are conditionals several levels deep.
* There are more than 7 parameters or variables in scope at a time.

### Writing Clean Classes

Classes are like headings in a book, there should be multiple layers of abstraction going from high level ideas to more detailed lower level ideas:

* Chapter
    * Heading 1
        * Paragraph 1
        * Paragraph 2
    * Heading 2
        * Paragraph 1

<br>
* Module
    * Class 1
        * Method 1
        * Method 2
    * Class 2
        * Method 1

#### High Cohesion

Cohesion is the fact of forming a united whole. When a class is said to have high cohesion, all of its functionality is closely related. We should strive to create classes with high cohesion. High cohesion not only enhances readability; it also increases the likelihood of reusing the class. Signs that a class does not have high cohesion are:

* The class has methods that don't interact with the rest of the class.
* The class has fields only used by one method.
* The class changes often.

For example:

```python
# Low cohesion class
class Vehicle(object):

    def edit_options():
        pass

    def update_pricing():
        pass

    def schedule_maintenance():
        pass

    def send_maintenance_reminder():
        pass

    def select_financing():
        pass

    def calculate_monthly_payment():
        pass
```

The Vehicle class contains many unrelated methods. This makes it harder to use and maintain because parts of unrelated code are intertwined together. A better approach would be to split this class up into smaller classes:

```python
# High cohesion classes
class Vehicle(object):
    def __init__(self)

    def edit_options():
        pass

    def update_pricing():
        pass

class VehicleMaintainer(object):
    def schedule_maintenance():
        pass

    def send_maintenance_reminder():
        pass

class VehicleFinancer(object)
    def select_financing():
        pass

    def calculate_monthly_payment():
        pass
```

#### Method Proximity

Code should read top to bottom and related methods should be kept together:

```python
def add_take():
    if not validate_take():  # First method referenced should be directly below
        raise ValueError('Take is not valid')
    save_take()  # Second method referenced should be below first

def validate_take():
    return take.endswith(‘.mov’)

def save_take():
    # save in database
```

Collapsed code should read like an outline. Strive for multiple layers of abstraction:

* Class
    * Method 1
        * Method 1a
            * Method 1ai
            * Method 1aii
        * Method 1b
        * Method 1c
    * Method 2
         * Method 2a

### Comments

Comments should only be used to explain ideas and assumptions not already apparent by reading the code.

#### Redundant Comments

The comments in this code do not add anything the user could not have figured out by reading the code.

```python
# Clear the node combo box then add items
self.node_combobox.clear()
if nodes:
    # Sort the nodes
    nodes.sort()
    # Check to see if there is a shape controller associated with the node
    self.find_shape_controllers(nodes)

    # Now add the list of nodes to the combo box
    self.node_combobox.addItems(nodes)

    # If a node is specified set the combo box
    if node:
        # Find the node's index
        index = self.node_combobox.findText(
            node,
            QtCore.Qt.MatchExactly | QtCore.Qt.MatchCaseSensitive)
        self.node_combobox.setCurrentIndex(index)
```

#### Divider Comments

If you see divider comments, it's a sign you need to extract the code into its own function:

```python
# Now create the new group object and insert it into the table
# ----------------------------------------------------------------------------------
# Create the group object
group = slidergroup.SliderGroup(name)
self._slider_groups[name] = group
# Tell the group what its start row is
group.setRow(row)        
# Apply color
if color:
    group.setColor(color)        

# Generate sliders from the attributes attached to the group
# ----------------------------------------------------------------------------------
row_index = row + 1
sliders_to_add = []
for attr in attributes:
    if cmds.objExists(attr):
        slider = self.add_slider(attr, rowIndex, group)
        row_index += 1
```
