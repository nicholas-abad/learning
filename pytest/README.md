# Introduction to PyTest

Video Link: [https://www.youtube.com/watch?v=cHYq1MRoyI0&amp;ab_channel=freeCodeCamp.org](https://www.youtube.com/watch?v=cHYq1MRoyI0&ab_channel=freeCodeCamp.org)

- General
    - Auto-discovery of tests
    - Clean reports when your test fails
    - Supports parameterized and fixture-based testing
        - Can easily run a test multiple times with different arguments
- Usage
    - Create a script with functions (e.g. `source/my_functions.py`)
    - Create another script that contains tests (e.g. `tests/test_my_functions.py`)
    - Call PyTest on the testing script in the command line (e.g. `pytest tests/test_my_functions.py`)
- Examples:
    - ## Function-based Tests
        - ### 1. Basic Example:
            - **Addition Function**
            ```
            def add(number_one, number_two):
                return number_one + number_two
            ```
            - **Test**
            ```
            def test_add():
                result = my_functions.add(1, 4)
                assert result == 5
            ```
        - ### 2. Throwing an error
            - **Division Function**
            ```
            def divide(number_one, number_two):
                return number_one / number_two
            ```
            - **Test**
            ```
            def test_divide_by_zero():
                with pytest.raises(ZeroDivisionError):
                    my_functions.divide(10, 0)
            ```
    - ## Class-based Tests
        - Can use the `-s` flag in `pytest` to see the setup/teardown
        - ### 1. Basic Example:
            - Class
            ```
            import math

            class Shape:
                def area(self):
                    pass
                def perimeter(self):
                    pass
                
                
            class Circle(Shape):
                def __init__(self, radius):
                    self.radius = radius
                    
                def area(self):
                    return math.pi * self.radius ** 2
                
                def perimeter(self):
                    return 2 * math.pi * self.radius
            ```
            - Testing
            ```
            class TestCircle:
                def setup_method(self, method):
                    print(f"Setting up {method}")
                    self.circle = shapes.Circle(10)
                def teardown_method(self, method):
                    print(f"Tearing down {method}")
                    del self.circle
                    
                def test_radius(self):
                    assert self.circle.area() == math.pi * self.circle.radius ** 2
                    
                def test_perimeter(self):
                    assert self.circle.perimeter() == 2 * math.pi * self.circle.radius
            ```
- PyTest Fixtures
    - Fixture is an object that can be used in multiple tests (e.g. create an instance of a class once that's used in multiple different tests)
    - These fixtures can be globally put into `conftest.py`
    - Example of `conftest.py`
        - ```
        @pytest.fixture
        def my_rectangle():
            return shapes.Rectangle(10, 20)

        @pytest.fixture
        def weird_rectangle():
            return shapes.Rectangle(5, 6)
        ```
    - Now, when defining a test, you could put in `weird_rectangle` as an input parameter and it'll load in that rectangle.

- Marking
    - Marks let you add metadata to your test
    - Could mark tests as being slow or to skip
    - Example of marking a test as **slow**:
        ```python
        @pytest.mark.slow
        def test_very_slow():
            time.sleep(5)
            result = my_functions.divide(10, 5)
            assert result == 2
        ```
    - In the terminal, you could then do `pytest -m slow` to only run the slow tests
    - Example of marking a test as **skip**:
        ```python
        @pytest.mark.skip(reason="This feature is currently broken")
        def test_add():
            assert my_functions.add(1,2) == 3
        ```
    - Automatically is skipped when running `pytest`
    - Example of marking a test such that it will fail using **xfail**:
        ```python
        @pytest.mark.xfail(reason="We know that we cannot divide by zero")
        def test_divide_zero_broken():
            my_functions.divide(4, 0)
        ```
    - Can use it when you know the test is going to fail.

- Parameterizing
    - Rather than using a for-loop to test different values, you can parameterize.
        ```python
        @pytest.mark.parameterize(
            "side_length, expected_area", 
            [(5, 25), (4, 16), (9, 81)]
        )
        def test_multiple_square_areas(side_length, expected_area):
            assert shapes.Square(side_length).area() == expected_area
        ```

- Mocking
    - Let's say that you have a function that calls an API
    - If you don't want to call the API every single time you run a test, you could mock it such that you get a known response
    ```python
    import pytest
    import source.service as service
    import unittest.mock as mock

    @mock.patch("source.service.get_user_from_db")
    def test_get_user_from_db(mock_get_user_from_db):
        mock_get_user_from_db.return_value = "Mocked Alice"
        user_name = service.get_user_from_db(1)
        
        assert user_name == "Mocked Alice"
    ```
    - NOTE: Watch another video or read more about this. Doesn't quite click yet.

- 


