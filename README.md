# code-fixing-using-langChain
This repository contains a Python Notebook that uses OpenAI's GPT-3 API to create a chatbot that can help fix Python code that does not pass certain test cases.

## Usage

To use the chatbot, run the `Fixing Code using Langchain.ipynb` script in a Python environment with the required libraries installed. The notebook requires an OpenAI API key to be set as an environment variable `OPENAI_API_KEY`.

The notebook defines a function `fix_code(code, n, test_cases)` that takes in three arguments:

- `code`: a string containing the code to fix
- `n`: an integer specifying the maximum number of attempts to fix the code
- `test_cases`: a list of strings, where each string is a test case that the code should pass

The function attempts to fix the given code by sending a message to the chatbot with the original code, the error message that is returned when the code is run with the given test cases, and the format instructions for the chatbot's response. The chatbot will respond with a fixed version of the code that passes the test cases, along with an explanation of the changes made to the code.

If the chatbot is unable to fix the code within `n` attempts, the function will return `-1` to indicate failure.

## Example

Here is an example of how to use the `fix_code` function:

```python

code = """def can_rearrange(s1, s2):
    # If the strings are identical, then s1 can be rearranged to be s2
    s1=s
    if s1 == s2:
        return True
    else:
        return False"""
error = ""

#the test cases have cases where the function should account for letters case insensitivity like "Race" and "care"
test_cases=["assert can_rearrange('Race', 'care')==True",
           "assert can_rearrange('hello', 'world')==False",
           "assert can_rearrange('lIsten', 'silent')==True"]

fixed_code, explanation = fix_code(code, 5, test_cases)
print(explanation)
print(f"\n\033[32m{fixed_code}\033[0m")
```
## This will output:
```output
def can_rearrange(s1, s2):
    # If the strings are identical, then s1 can be rearranged to be s2
    s1=s
    if s1 == s2:
        return True
    else:
        return False

Traceback (most recent call last):
  File "/tmp/ipykernel_84/3013963956.py", line 9, in check_code
    exec(test_case)
  File "<string>", line 1, in <module>
  File "<string>", line 3, in can_rearrange
NameError: name 's' is not defined

##################################################################
Fixing

Trial #0
Explanation: 
Fixed the NameError by removing the line that assigns s1 to s. Also, changed the logic to sort the strings and compare them ignoring case.
Success
def can_rearrange(s1, s2):
    # If the strings are identical, then s1 can be rearranged to be s2
    if sorted(s1.lower()) == sorted(s2.lower()):
        return True
    else:
        return False
```
## The fixed code:
```python
def can_rearrange(s1, s2):
    # If the strings are identical, then s1 can be rearranged to be s2
    if sorted(s1.lower()) == sorted(s2.lower()):
        return True
    else:
        return False
```
