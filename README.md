# My Unit Test Class

## Table of Contents

- [Introduction](#introduction)
- [Setup](#setup)
- [Example](#example)

## Introduction

This class has been written in order to develop applications using Test-Driven Development (TDD).

As for now it simply implements:

- `assert_true`
- `assert_false`

but there might be soon more features.

## Setup

In order to use this class, just grab the two files `MyTest.cpp` and `MyTest.h` and put them into your project.

These are the steps one needs to follow in order to make use of this test class:

1.  Create the test object, passing the title of the test:

        MyTest test("Session of tests Title");

2.  Define the `.setup` attribute, which will be a `std::function<void()> `, and can be set using a lambda function:

        test.setup = [] {};

    This function will be used before every test - a part from the first, and this is why we need to call it immediately afterwards (if something has to be done in the setup):

3.  Call the `.setup` :

        test.setup();

4.  Add tests to the `test` object where a test is, for example, `test.assert_true( "Test Title", "Error You would like to display if the test fails", std::function<bool()> function )` :

        test.add_test(test.assert_true(
            "This is the test title",
            "Error to be shown if test does not pass",
            [] {
                return false;
            }
        ));

5.  Run tests:

        test.run();

    This will:

    - print the title of the tests' session;
    - run all the tests and collect the failing ones;
    - display failing ones with the error messages;
    - display the overall number of tests passed with respect to the total.

## Example

Consider the case I have a Max Heap class and I would like to test whether the method `get_max()` returns the correct maximum value in the heap (namely the root). In the main function I define:

        MaxHeap max_heap;

        MyTest test("Testing Max Heap Class");

        test.setup = [&max_heap] {
            //Reset the max_heap before each test;
            MaxHeap _max_heap;
            max_heap = _max_heap;
        };

        test.add_test(test.assert_true(
            "Testing whether the get_max method returns the biggest element in the Heap",
            "get_max should return the maximum value",
            [&max_heap] {
                max_heap.insert(10);
                max_heap.insert(20);
                max_heap.insert(15);

                return max_heap.get_max() == 20;
            }
        ));
        test.run();
