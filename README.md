### Integration of a Mathematical Calculations with a Chat Completion System using LLM Function-Calling
### AIM

To design and implement a Python function for calculating the volume of a cylinder and integrate it with a chat completion system utilizing the function-calling feature of a Large Language Model (LLM).

### PROBLEM STATEMENT

Traditional Python programs can perform mathematical calculations but cannot naturally understand human language queries. Large Language Models (LLMs) can understand user requests in natural language but cannot directly execute Python functions.

The objective of this experiment is to integrate a mathematical calculation function with an LLM-based chat completion system using function-calling. The system should allow the LLM to identify the appropriate function, extract the required parameters, and enable Python to execute the actual mathematical computation.

The mathematical operation implemented in this experiment is the calculation of the volume of a cylinder.

The formula used is:
      ### V=πr2h
Where:

r = radius
h = height

### DESIGN STEPS
### STEP 1:

Create a Python function to calculate the volume of a cylinder using radius and height as inputs.

### STEP 2:

Define the function metadata/schema and provide it to the Large Language Model (LLM) using the function-calling feature.

### STEP 3:

Send the user query to the LLM, extract the function name and arguments returned by the model, and execute the corresponding Python function to generate the result.

### PROGRAM
```
import os
import openai
import json
import math

from dotenv import load_dotenv, find_dotenv

# Load API Key
_ = load_dotenv(find_dotenv())

openai.api_key = os.environ['OPENAI_API_KEY']


# Python Function
def calculate_cylinder_volume(radius, height):

    volume = math.pi * radius * radius * height

    return volume


# Function Metadata for GPT
functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate the volume of a cylinder",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "number",
                    "description": "Radius of the cylinder"
                },
                "height": {
                    "type": "number",
                    "description": "Height of the cylinder"
                }
            },
            "required": ["radius", "height"]
        }
    }
]


# User Message
messages = [
    {
        "role": "user",
        "content": "Calculate cylinder volume with radius 3 and height 10"
    }
]


# Chat Completion Request
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call="auto"
)


# Extract Response
message = response["choices"][0]["message"]


# Execute Function
if message.get("function_call"):

    function_name = message["function_call"]["name"]<img width="1353" height="417" alt="exp1 output" src="https://github.com/user-attachments/assets/1caabfcf-7c01-4a37-a965-4641597d4be2" />


    arguments = json.loads(
        message["function_call"]["arguments"]
    )

    result = calculate_cylinder_volume(
        arguments["radius"],
        arguments["height"]
    )
    print("Name: Narendran K\n Register Number : 212223230135")
    print("Volume of Cylinder =", result)
```
### Output:
![alt text](<exp1 output.png>)

### RESULT
Thus, the Python function for calculating the volume of a cylinder was successfully integrated with a chat completion system using the function-calling capability of a Large Language Model (LLM). The LLM successfully identified the appropriate function and extracted the required parameters from the user query, while Python executed the actual mathematical computation and generated the final result.