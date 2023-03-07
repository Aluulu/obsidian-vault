Functions is a combination of code that is intended to be ran multiple times. In [[Python]], functions have very specific rules they must follow.

## Syntax

The standard for writing function names are written using snake_case and lowercase. This is denoted in PEP8:

> Function names should be lowercase, with words separated by underscores as necessary to improve readability

\-https://peps.python.org/pep-0008/#function-and-variable-names

```Python
def <function_name>(<function_arguments>):
	# Function contents are tabbed
```

### Simple example

```Python
def main():
	print("This is a function")
	another_function()

def another_function()
	print("This is another function")
```

### Detailed example

```Python
def add_two_numbers(x, y):
	z = x + y
	return z
```

## Function returns

To return a value from a function, you can do so using the `return` command like so:

```Python
def <function_name>(<argument_1>, <argument_2>):
	# Function contents
	return <value_to_be_returned>
```

To then use that function, simply call the function and either store it as a variable or use it in a line like so:

```Python
def <function_name>(<argument_1>, <argument_2>):
	# Function contents
	return <value_to_be_returned>

def main():
	<returned_value> = <function_name>(<argument_1>, <argument_2>)
	# Do stuff with the variable
```

### Simple example

```Python
def main():
	returned_value = add_two_numbers(5, 6)
	print(returned_value)

def add_two_numbers(x, y):
	value_to_return = x + y
	return value_to_return
```

### Using a return value in another line

```Python
def main():
	new_value = 4 + add_two_numbers(5,6)
	print(new_value)	

def add_two_numbers(x, y):
	value_to_return = x + y
	return value_to_return
```

## Return multiple values

You can return multiple values by simply adding a comma to the last value to return and adding another value to be returned; like so:

```Python
def <function_name>:
	# Function contents
	return <return_value_1>, <return_value_2>
```

### Example

```Python
def main():
	original_value, original_value_plus_ten = add_two_numbers_add_ten(5,6)
	print(original_value)	
	print(original_value_plus_ten)

def add_two_numbers_add_ten(x, y):
	arguments_added = x + y
	added_values_to_return = arguments_added + 10
	return arguments_added, added_values_to_return
```

## Function arguments

If you want to pass arguments to a function, you can do so using the following syntax:

```Python
def <function_arguments>(<argument>):
	# Function contents
```

### Example

```Python
def main():
	introduce_person("Callum")

def introduce_person(name):
	print("Hello! My name is: ", name)
```

## Function docstrings

Function docstrings are documentation that explains everything you will need to know about a function. 

Function docstrings use a single hastag `#` to denote documentation. These comments can be accessed by using the command `<function_name>.__doc__`.

```Python
print(<function_name>.__doc__)
```

The syntax for dostrings are specified in the PEP 257:

> Multi-line docstrings consist of a summary line just like a one-line docstring, followed by a blank line, followed by a more elaborate description.
> The docstring for a function or method should summarize its behavior and document its arguments, return value(s), side effects, exceptions raised, and restrictions on when it can be called (all if applicable).

\- https://peps.python.org/pep-0257/

### Example

```Python
def add_two_numbers(x, y):
    '''
    Returns the sum of two integers.

            Parameters:
				x (int): The first integer to be added
				y (int): The second integer to be added

            Returns:
				z (int): The sum of x and y
    '''
    z = x + y
    return z
```