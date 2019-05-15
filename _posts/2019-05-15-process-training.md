---
layout: post
title:  "Training a Machine Learning algorithm with data from the DCD Hub in Python"
date:   2019-05-15 01:30:13 +0000
categories: Process
tags: ML Label Data Python
---

In this post we train a machine learning algorithm with data from the DCD Hub,
in Python. We 

# Train

We use a Jupyter Notebook to lead you through the training process.

If you do not have Jupyter yet, you can install it with the following commands
in the terminal:

```bash
python3 -m pip install --upgrade pip
python3 -m pip install jupyter
```

Then, you can start with:

```bash
jupyter notebook
```

Running this command should redirect you to your favorite web browser and open
the Jupyter web interface. Otherwise, a link shows up in the terminal, you can
copy/paste it in your web browser.

Downloads this Jupyter example to start exploring the your data (right-click and 'Save link As...'):
[training.ipynb](https://github.com/datacentricdesign/docs/raw/master/examples/process/training.ipynb)

Back in Jupyter, click on File > Open

![Jupyter Open](/docs/assets/res/jupyter-open.png)

Then, click on Upload

![Jupyter Upload](/docs/assets/res/jupyter-upload.png)


# Predict

Once you trained your classifier and save its model in a file, you are ready to
use it for prediction.

```python
from dotenv import load_dotenv
import os
import pickle
import serial
import numpy as np

from dcd.entities.thing import Thing

# The thing ID and access token
load_dotenv()
THING_ID = os.environ['THING_ID']
THING_TOKEN = os.environ['THING_TOKEN']

# Name of the property containing the labels
CLASS_PROP_NAME = "Sitting Posture"

# Where to read the model from
MODEL_FILE_NAME = "model.pickle"


# Instantiate a thing with its credential
my_thing = Thing(thing_id=THING_ID, token=THING_TOKEN)
my_thing.read()

# Extract labels from the label property
sitting = my_thing.find_property_by_name(CLASS_PROP_NAME)
classes = []
for clazz in sitting.classes:
    classes.append(clazz['name'])

# Load the classifier (model trained in the previous section)
with open(MODEL_FILE_NAME, 'rb') as file:
    neigh = pickle.load(file)

# Read data from serial port
ser = serial.Serial(
    port=os.environ['SERIAL'],
    baudrate=9600,
    timeout=2)

# Make a prediction and show the label of the predicted class
def predict(values):
    result = neigh.predict(values)
    print(classes[result[0]])

# Real time prediction
def serial_to_property_values():
    line_bytes = ser.readline()
    # If the line is not empty
    if len(line_bytes) > 0:
        # Convert the bytes into string
        line = line_bytes.decode('utf-8')
        str_values = line.split(',')
        if len(str_values) > 1:
            str_values.pop(0)
            values = [float(x) for x in str_values]
            values = [values]
            np.array(values).reshape(1, -1)
            predict(values)


while True:
    serial_to_property_values()
```
