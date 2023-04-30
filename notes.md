# Python Tutorial: Unit Testing Your Code With the unittest Module

## What is Unit Testing?
- Unit Testing is a type of software testing where individual units or components of a softwar  e are tested.
- Unit testing helps us to save time and problems down the road. 
- Writing good tests means that our updates and refactoring do n't have any unintended consequences and don't break our code.

## Starting to create Unit tests
> Printing results can be a little bit lame, can't it?

We have the next file called `calc.py` with the next functions:
```python
def add(x, y):
    """Add Function"""
    return x + y


def subtract(x, y):
    """Subtract Function"""
    return x - y


def multiply(x, y):
    """Multiply Function"""
    return x * y


def divide(x, y):
    """Divide Function"""
    if y == 0:
        raise ValueError('Can not divide by zero!')
    return x / y
```

We'd like to know if our functions are written correctly, even when you make changes to it, so, in order to create **Unit Tests** we need to follow the next steps:

1. Create a new file called `test_calc.py`. It's recommended to always start your dile with `test` and then the file you are goint to test.

2. Inside `test_calc.py` you need to import the `unittest` module. Add the next line to your code:
    ```python
    import unittest
    import calc
    ```
    Don't forget to also import `calc`! This is where our functions are in order to test them.
  
3. Create a new class (You can name it however you like, but try to keep it descriptive) and inherit from `unittest.TestCase`:
    ```python
    class TestCalc(unittest.TestCase):
    ```
    - `unittest.TestCase` Is going to give us access to a different testing capabilites within that class. These are just an example:
    |Method|Checks that|
    |------|-----------|
    |`assertAlmostEqual(a, b)`|`round(a-b, 7) == 0`|
    |`assertNotAlmostEqual(a, b)`|`round(a-b, 7) != 0`|
    |`assertGreater(a, b)`|`a > b`|
    |`assertGreaterEqual(a, b)`|`a >= b`|
    |`assertLess(a, b)`|`a < b`|
    |`assertLessEqual(a, b)`|`a <= b`|
    |`assertRegex(s, r)`|`r.search(s)`|
    |`assertNotRegex(s, r)`|`not r.search(s)`|
    |`assertCountEqual(a, b)`|*a* and *b* have the same elements in the same number, regardless of their order|

4. Now we can write our first test inside `TestCalc` testing our `add` function:
    ```python
    class TestCalc(unittest.TestCase):

      def test_add(self):
        result = calc.add(10,5)
        self.assertEqual(result, 15)

    ```
    - Every test we write **it needs to start with `test`** This is a required naming convention.
    - `assertEqual` checks whether `result` is equals to 15 or not.

5. In order to run our tests, we need to open the command line. Once we are in the directory where our files are, you run the next command:
    ```
      python test_calc.py
    ```
    But unfortunately we can't see any result. This is because we need to run unit tests as our main module and pass in `test_calc.py`. You can do it like this:
    ```
    python -m unittest test_calc.py
    ```
    And you'll get the next result:
    ```
    .
    ------------------------------------
    Ran 1 test in 0.000s

    OK
    ```
    That `OK` means that our tests ran correctly.

6. If you still want to still run the module using `python test_calc.py`, you need to add this next piece of code inside `tesy_calc.py`:
    ```python
    if __name__ == '__main__':
      unittest.main()
    ```
    - Basicaly, what we did here is that if we run this module directly, then run the code within the conditional, being the code `unittest.main()`, being our code written in `tesy_calc.py`.

7. Now we might add more tests in order to test some edge cases:
    ```python
    def test_add(self):
        self.asssertEqual(calc.add(10, 5), 15)
        self.asssertEqual(calc.add(-1, 1),0)
        self.asssertEqual(calc.add(-1,-1),-2)
    ```
    And we get:
    ```
    .
    ------------------------------------
    Ran 1 test in 0.000s

    OK
    ```
    Indicating that our function is working correctly

### Adding more test functions to `test_calc.py`

1. Add the next piece of code inside `TestCalc` class:
    ```python
    def test_substract(self):
        self.asssertEqual(calc.add(10, 5), 5)
        self.asssertEqual(calc.add(-1, 1),-2)
        self.asssertEqual(calc.add(-1,-1),0)

    def test_multiply(self):
        self.asssertEqual(calc.add(10, 5), 50)
        self.asssertEqual(calc.add(-1, 1),-1)
        self.asssertEqual(calc.add(-1,-1),1)

    def test_divide(self):
        self.asssertEqual(calc.add(10, 5), 2)
        self.asssertEqual(calc.add(-1, 1),-1)
        self.asssertEqual(calc.add(-1,-1),1)
    ```

2. In order to see what happens when a test fails, go to `calc.py` and change the `multiply` function in this way:
    ```python
    def multiply(x, y):
        """Multiply Function"""
        return x ** y
    ```
    Now `multiply` will return x^y, instead of x*y.

3. Now, if we run again our terminal with `python test_calc.py`, we'll get:
    ```
    ..F.
    ==================================================================
    FAIL: test_multiply (__main__.TestCalc)
    ------------------------------------------------------------------
    [...]
    AssertionError: 100000 != 50
    ------------------------------------------------------------------
    Ran 4 tests in 0.001s

    FAILED (failures=1)
    ```
    Indicating that one test failed.

4. Change the `multiply` function again to its correct version.

5. If you watch the `divide` function, you'll notice that if `y` equals zero, it'll raise an error. You cant test this error by two ways:

    1. Use the `self.assertRaises()` function in the next way (Inside `test_divide`):
        ```python
        self.assertRaises(ValueError, calc.divide, 10, 0)
        ```
        - `ValueError` means the exception we are expecting.
        - `calc.divide` is the function we are going to test.
        - `10, 0` are the arguments we are passing to the function.

    2. Use a context manager that will allow you to handle and check the exception properly and let you call the function properlly:
        ```python
        with self.assertRaises(ValueError):
            calc.divide(10,0)
        ```
        By now, we'll use the second way

## Writing slightly more difficult tests


