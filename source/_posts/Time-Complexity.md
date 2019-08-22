---
title: Time-Complexity
date: 2019-08-20 04:50:40
tags:
  - cs fundamentals
  - algorithms
  - time complexity
  - data structures
---

## Introduction to Big O Notation and Time Complexity

Today, I'm we will learn about Big-O notation and time complexity.
These tools will help you measure the time needed to execute a given program.
The time needed to execute a program depends on many factors like CPU speed and Rate of write and read from your desk.

But this is not a reliable method of assessing how good your algorithms
a better way is to measure how the time consumed increases as the input size grows.
Visually, it looks like this.

{% asset_img time-complexity.png graph showing run time on y-axis and input-size one x-axis and linear relationship between then %}

To better understand this, let's look at an example.

```cpp
int sum(int a, int b) {
  return a + b;
}
```

The above program will do a summation, and then returns
regardless of how big the inputs are so, you can say that the run time
of such algorithms is constant in relation to the input size.

{% asset_img constant.png graph showing y-axis representing runtime parallel to x-axis representing input size %}

Now, consider that instead of, summing two numbers you want to sum a list of integers.

```cpp
int sum(vector<int> numbers)
{
  int sum = 0;
  for (int i=0; i<numbers.size(); ++i) {
    sum += numbers[i];
  }
  return sum;
}
```

this program will always do

1. Initialize an int (sum) and this will cost 1 operation
2. for all the elements in the numbers list, it adds the current element to the sum
   which cost will be equal to the sum operation \* size of numbers list.
3. finally, it returns

so you can say that totalCost = 1 + size(numbers) + 1
this will be a linear relationship between the input size and time needed to execute the program
you can see that there a single variable the affects the relation more than anything else.

and for this reason, we use asymptotic run time as we don’t care how many steps exactly, the program will take but you need to describe the relationship between the increase in input and run time.

## Big O, Big theta and Big Omega

**O (big O)**
It concerns the worst-case scenario for a given algorithm as it represents the upper bound on the run time,
so technically speaking you can say that for the sum program the run time is O(sizeOfList) or O(sizeOfList^2)
think of as **runTime <= O()**
but this not useful or accurate actually as you try to find the closest fit when doing
this type of analysis.

**&Omega; (big omega)**
It represents a lower pound on run time or **runTime >= &Omega;**
in plain English &Omega; means the algorithm will never be faster than the given function
and O means the algorithm will never run slower than the given function

**&theta; (big theta)**
It combines the two meaning your run time will not run slower or faster than the given bounds
this can be expressed as **&Omega; <= runTime =< O**

## Space complexity

Time is not the only metric we care about when analyzing an algorithm performance.
Memory usage is also a very important metric to know.
We use the same notation and ideas to express space complexity,
for example, to create a list of size n this will consume n units of memory.
Stack count in recursive algorithms also counts but,
please be careful as not every call to a function counts as an addition to the stack.
As if a function returns before the next step its stack gets destroyed
so if you called this function n times in your program the space complexity will still be constant.

e.g

```cpp
  int sumTwoNumbers(int a, int b) {
    return a + b;
}

int sum(vector<int> numbers) {
    int sum = 0;
  for (int i=0; i<numbers.size(); ++i) {
    // it will get called n times but it's stack get destroyed after it returns
    sum = sumTwoNumbers(sum, numbers[i]);
  }
  return sum;
}
```

But if the stacks co-exists the space complexity will be linear

```cpp
int recursiveSum(int n) {
  if(n <= 0) {
    return 0;
  }
  return n + recursiveSum(n-1);
}
```

In the example above the sum, functions will keep adding to the stack
till it hits the base condition and then it will start removing the stack one by one.
