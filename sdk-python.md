---
layout: page
title: "Python SDK"
---

The Python SDK facilitate the interaction with the Data-Centric Design Hub.

# Getting Started

If your Python 3 (not Python 2) environment is ready, you can directly skip to step 3.

## Step 1: Setting up Python 

Select, download and install the latest version of Python 3 for your system
<a href="https://www.python.org/downloads/release/python-372/" target="_blank">here</a>.

<details><summary markdown="span">On Windows</summary>

Once installed, go to 'Start > System > Properties > Advanced System Properties >
Environment Variable' In User Variables, double click on 'Path'. At the end of
the line, add a semi-colon (;<zero-width space>), followed by:

C:\Users\YOUR_USERNAME\AppData\Local\Programs\Python\Python37;C:\Users\YOUR_USERNAME\AppData\Local\Programs\Python\Python37\Scripts

(Replace YOUR_USERNAME with your Windows user name)

Open the Command Prompt to check the installation, by typing in your console:

```bash
python --version
```

And verifying that the correct version of python was installed.

</details>

<details><summary markdown="span">On Mac and Linux</summary>

After installation, open the Terminal to check if it was successful, by typing
the following on your console:

```bash
python3 --version
```

If the correct version of python is shown, the install was successful.

</details>


## Step 2: Python Dependencies

In the Python ecosystem, Pip is a tool that manages packages for us. We will use
it to install and update any Python library our project relies on. You can check
whether Pip is already install with the following command.

__On Windows__, type in:

```bash
python -m pip --version
```

If it is not found, download the file <a href="https://bootstrap.pypa.io/get-pip.py" target="_blank">get-pip.py</a>
and save it (CMD+S or Ctrl+S) in your Downloads folder. In the Atom terminal, type in the
following command:

```bash
python Downloads\get-pip.py
```

__On Mac and Linux__, type in:

```bash
python3 -m pip --version
```

If it is not found, you can install it as follows.

```bash
python3 get-pip.py
```

## Step 3: Python in Atom

Atom is a software to edit your code, referred to as Integrated Development Environment.
Click [here] if you need to install Atom. The next step is the Python plugin for
Atom, to get some help specifically for Python in Atom. Go to the terminal and type:

__On Mac and Linux__, type in:

```bash
python3 -m pip install 'python-language-server[all]'
```

__On Windows__, type in:

```bash
python -m pip install 'python-language-server[all]'
```

When it is installed, on the top menu of Atom, click on *'Packages' >
'Settings View' > 'Install Packages/Themes'*. Search and install 'atom-ide-ui'
and 'ide-python'.

If you do not have experience with Python, we recommend you to go through the
following tutorial to get started:
<a href="http://www.learnpython.org/" target="_blank">http://www.learnpython.org/</a>


## Step 4: Dependencies

We use Pip to install the dependencies we need, listed in the file requirements.txt.
In Atom, right click at the root of your project (left panel), create a file
'requirements.txt' and type in the following line.

```txt
dcd-sdk>=0.0.18
paho-mqtt
python-dotenv
pyserial
requests
```

This is the dependence to Python SDK of the Data-Centric Design Hub.

Open the Atom terminal ('plus' sign in the bottom-left corner) and execute the following command.

__On Windows__, type in:

```bash
python -m pip install -r requirements.txt --user
```

__On Mac and Linux__, type in:

```bash
pip3 install -r requirements.txt --user
```

Here we 'install' the Python dependencies for our project. The option -r indicates
we provide a file name that contains the required dependencies, the option --user
indicates we install the dependencies in a dependency folder specific for the
current users.

## Step 5: Connecting a Thing to the Hub

At this stage you need the credentials of the [Thing](/docs/api.html#Thing) you 
want to connect to the hub. If you do not have one yet, please sign in/sign up to
the DCD Hub and create a [Thing](/docs/api.html#Thing) following the instructions
[here](/docs/api.html#sign-up).

In Atom, right click at the root of your project (left panel) and create a file
'random-data.py'.

In this file, add the following lines to import the definition of a 
[Thing](/docs/api.html#Thing) and [PropertyType](/docs/api.html#property-types)
from the Python SDK.

```python
from dcd.entities.thing import Thing
from dcd.entities.property import PropertyType
```

Then, we set the credential of our [Thing](/docs/api.html#Thing).
In Python, it means we look at the environment variables to read the id and
access token of our thing. To provide these information as environment variable,
right click at the root of your project (left panel) and create a file '.env'.

In this file, type in the following and paste your id and access token after
the equal signs.

```bash
THING_ID=
THING_TOKEN=
```

Note: If your are using [Git](/docs/2019/04/30/tools-git), you do not want to track
the file '.env' with Git as it contains secrets. To avoid any mistake, the file
.gitignore list all files, folders and extensions to ignore. Create a file '.gitignore'
and add a new line with '.env'.

Back into our python file, we can now import our credential. We load environment
variables and access our id and token as follows:

```python
from dotenv import load_dotenv
import os
```

```python
# The thing ID and access token
load_dotenv()
THING_ID = os.environ['THING_ID']
THING_TOKEN = os.environ['THING_TOKEN']
```

Note: In Python, any line starting with a '#' is a comment, to help understand
what the code does but ignored by Python when running the programme.

Next, we can instantiate a [Thing](/docs/api.html#Thing) with the credentials.
We store this object in a variable called 'my_thing', which we will use to manage
our Thing on the DCD Hub.

```python
# Instantiate a thing with its credential
my_thing = Thing(thing_id=THING_ID, token=THING_TOKEN)
```

The following line 'read' the details of our Thing, meaning it connects the DCD Hub
and asks for the information related to this Thing.

```python
# We can fetch the details of our thing
my_thing.read()
```

We can use the method to_json() to get a [JSON](https://json.org/) view of the
Thing. We show the result in the console with the Python function print(). If
you just registered your Thing on the DCD Hub, it has only an Id, a name and a type.

```python
print(my_thing.to_json())
```

To create a [Property](/docs/api.html#Property) for our [Thing](/docs/api.html#Thing),
we can use the method find_or_create_property(). This method takes a property 
name and a property type as parameters, search for a property of the same name
in the Thing, and return the property. If no property is found, it requests the 
creation of a new one on the DCD Hub and returns it. In the following example,
we create a property with the name 'My Random Property' of type 'THREE_DIMENSIONS',
meaning that every data point will be compose of three values.

```python
# If we have no properties, let's create a random one
my_property = my_thing.find_or_create_property("My Random Property",
                                               PropertyType.THREE_DIMENSIONS)
```

Similar to the [Thing](/docs/api.html#Thing), we can display the details of a 
[Property](/docs/api.html#Property) with the method to_json().

```python
# Let's have a look at the property, it should
# contains the name, a unique id and the dimensions
print(my_property.to_json())
```

## Step 6: Execute the Python code

Let's execute this code. Go to the Atom terminal and type in the following command:

**On Windows**, type in:

```bash
python random-data.py
```

**On Mac and Linux**, type in:

```bash
python3 random-data.py
```

If the example runs properly you should see a log generated every two seconds,
indicating dumb data is being sent to the Hub.

## Step 5: Sending Data

With this code we are ready to send data to the DCD Hub. To send random data,
we add two library at the top of the file, the first for generating random numbers,
the second to get time functionalities such as sleep() (pausing the programme).

```python
from random import random
import time
```

Then, we create a function that send random values to the Hub. In Python we do this
with the keyword 'def' followed by the name of the method and the parameters between
parenthesis. In this function we create Dictionary with three random values and we call
the method update_values(). This method prepare and send the dictionary of data to
the Hub.

```python
# Let's create a function that generate random values
def generate_dum_property_values(the_property):
    # Define a tuple with the current time, and 3 random values
    values = (random(), random(), random())
    # Update the values of the property
    the_property.update_values(values)
```

Finally, we can call this methods infinitely (While True), waiting 2 seconds after
each update. 

```python
# Finally, we call our function to start generating dum values
while True:
    generate_dum_property_values(my_property)
    # Have a 2-second break
    time.sleep(2)
```

Note: Indentation is key in Python. Take the previous example of condition, the indentation
defines what is in the condition. Any following line aligned with the if would be
considered outside the condition.

You can execute the Python script again and check incoming data with
[DCD data subject](/docs/2019/07/31/tool-data-subject).

Back in the Atom terminal, stop your Python script with CMD+C (Ctrl+C).
