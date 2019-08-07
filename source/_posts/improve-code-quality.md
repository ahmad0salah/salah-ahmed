---
title: Improving Your code Unit Testing
date: 2018-07-22 03:09:33
tags:
  - clean code
  - TDD
  - testing
---

# Why Testing?

Before discussing **why** testing is useful and **how** to do it, let’s take a minute to define what unit testing actually is. “Testing”, in general, programming terms, is the practice of writing code that invokes the code it tests to catch bugs. It **DOES NOT **prove that code is correct. It just reports if the scenarios you had thought of are handled correctly.

## What kinds of bugs can we catch with testing?

- syntax errors are
- Logic Errors perhaps you forget to limit a while loop and you get IndexError raised.

All of these can be caught through careful testing.

Unit testing specifically tests a single block ‘functionality’ of code in isolation.

let’s say you have a simple function that sums two integers but you want to make sure that it works you can try by hand run it several times in your terminal or think of couple edge cases write simple test function like this

# Why would you waste your time on such thing?

1. you wouldn’t get all the edge cases from the first time

2. If you want to be able to change your code and experiment with it you and be sure you didn’t break anything, unit testing is your friend

3. testing forces to be more rigorous about your code, possibly revealing logical errors

4. encourages modular, more organized code, which is a good practice

5. you don’t need actually to write all of this mess to test your code python and almost all modern languages have tools in place to help you

## python

python is really pleasant when it comes to testing tools

as it has to main tools to test your code

- doctest testing
- “usual” unit test

## doctest

it is an amazing way to get started documenting your code and its structure helps other people get a more solid understanding of your code as you provide examples of what this function is exactly doing.

<script src="https://gist.github.com/salah-ahmed/38e64f2994aedb860cb34c006c1836db.js"></script>

this should fail and output something like this

```
Exception raised:
Traceback (most recent call last):
File “/home/ahmed/miniconda3/lib/python3.6/doctest.py”, line 1330, in __run
compileflags, 1), test.globs)
File “<doctest sum.sum[1]>”, line 1, in <module>
sum(‘0’, 0)
File “/home/ahmed/Desktop/career/interview_practice/py_code/generel/sum.py”, line 8, in sum
return a+b
TypeError: must be str, not int
**********************************************************************
1 items had failures:
1 of 2 in sum.sum
***Test Failed*** 1 failures.
```

## The “usual” Unit Test

as you might guessed this is not for exhaustive testing of all edge cases

but it’s a nice thing to for documentation and small functions

# Structure of A Unit Test

you generally will have your code module.py and test file test_module.py

<script src="https://gist.github.com/salah-ahmed/523525d3b6de37f9c5a5ae9d93ee6491.js"></script>

```
$ python -m unittest test_math.py
```

List of assertions you will find it handy

{% asset_img python-assertions-list.jpeg %}

# Fixing Code Properly

the purpose of writing unit tests is to take a step back and determine the best way to fix the issue. In this case, rather than adding an additional condition avoid adding glue that will fall apart later

make your first step after failing a test is going back to pen and paper and think carefully about your code logic and what changes that will eliminate the problem cleanly and efficiently

## Test Coverage

how do you know if you’ve tested everything in your code ?

One way to do is looking at each line of code and make sure that there’s a test that corresponds to it and every time you have an if or an l if, you need to test both branches of it, But this not a fun thing to do

Thankfully there’s an Python library out there named **coverage.py** it handles the tedious task of testing every single line and branch in your code and gives you back some amazingly useful information

## installation

- \$ pip install coverage

## Usage

- first make your app run automatically this is the easiest way to start with coverage

- \$ coverage run <test_file_name>

_nothing interesting will happen but when you run_

\$ coverage report

{% asset_img coverage-report-1.png %}
so as you can see **coverage **tells us that in the file math1.py there is 4 statements and we missed one of these statements this doesn’t necessarily that we missed a real case here but it’s nice flag to cheek again our work .

let’s see what we have missed to know whether we missed a test here or everything is fine.

\$ coverage report -m
{% asset_img coverage-report-2.png %}

so if we went to our file math.py at line 3 we missed a statement.
yep at line three we didn’t test the function sum so we should added

as you have seen testing for how much of your code is covered can reveal cases that you didn’t test for, sure in real life it wouldn’t that trivial but if you applied systematic process it should be very easy to test and debug your code properly

# HTML Reports

a very nice feature of coverage that it can produce HTML reports which i find a lot more pleasant
than the command line version

\$ coverage html

{% asset_img coverage-html-1.png %}

and if you clicked on math1.py you will see
{% asset_img coverage-html-2.png %}

so here you go a way nicer looking interface it is not obvious from this example how useful this is but just imagine a dozen file with horrible coverage rate this will be really great help to find mistakes quickly.

## Testing web apps

there isn’t much here it’s the same mentality to test any type of application but as someone who wast just start learning i find it challenging to see what to test for so this light demo is here for people who are just getting started.

- assume that you have a web service built with flask and it simulates a dice roll.

you might something similar to this

Initially you would like to test for

- the values generated are actually correct
- the data is formatted properly

so you can write your test this way

let’s ignore flask and focus instead on the mental model

1. you will need to find the tools offered by the framework you choose
2. you should break down each chunk of your into path, request types
3. after separating each path break down the functionality to

- who the data is represented
- is data has the expected values

so as you see in the example above first i check that the data is in the expected range, then i check for the representation

_notice that this will give you a high coverage rate but there is a major flaw in the test take a second and think about it_

testing for a random value once is useless this test should be performed several times.

if we did follow the three steps we agreed on and after we tested the ‘web’ part of the script we stooped for a minute and thought about it the code and it’s correctness we would have avoided such mistake
