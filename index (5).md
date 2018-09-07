
# Debugging Python with `pdb`

### Objectives

* Understand the importance for debugging for software developers and data scientists.
* Use `pdb` and `ipdb` for debugging python code in jupyter and other IDEs.
* Create breakpoints in the code and inspect the variable state. 
* Step through the program to look for errors. 

### Background
For an absolute beginner, it is often difficult to write even a single line of code that works correctly. Most programming languages are very specific in terms of their syntax. You miss a set of brackets, or a colon/semi-colon or mis-spell a variable name and an error gets thrown back at you. As one moves towards writing non-trivial code in a  professional context, the level of code complexity increases accordingly and it still remains hard to write a few lines of code without tripping over. Identifying and correcting logical / semantic errors in a self-written program is a highly desirable skill for programmers as well as data scientists. 

![](http://www.oconne.com/onyxia/img/happiness_is.jpg)

We don't expect our code to work correctly the first time. Our code breaks all the time and we fix it - all the time. We build the code slowly, bit by bit. 

> **Programs aren't written. They are edited into existence**

In a professional context, debugging code (a process of tracking errors in code) may take more time than actually writing code.

The purpose of this lesson is to introduce the basics of `pdb`, the **"Python DeBugger"**. 
The lesson will provide you with some skills that will help run debugging sessions with ease. 

### What is a Debugger

A Debugger allows us to identify and remove bugs (errors) from our code as it let's us:

* Observe the internal state of a program during execution. 
* Test our code for presence of bugs before implementation stage. 
* Check our code's execution logic for identifying logical errors. 

Using a debugger, we can set a **breakpoint** at any line in our code to halt the execution and check for points shown above. 

#### and what is a Breakpoint ?

Creating breakpoints is a key feature for any debugger. This feature let's the debugger pause the program whenever a certain breakpoint in the program is reached. For each breakpoint, we can add further checks and conditions to monitor the program flow in finer details. We can set breakpoints to specify the place where our program should stop - a line number, function name or exact address in the program.

Debuggers are powerful tools and they can speed up the debugging process a lot faster than using simple `print()` statements in the code for viewing the state of variables. It normally takes some time before one feels comfortable in a debugging environment. The purpose of this lesson is to get your feet wet before you start using one in your data future science projects.

### `pdb` - the Python DeBugger

Python's comes with it's very own debugger, the `pdb`. The debugger is included in python, ready to be used in the same way any other library is imported and used. To see this in action, we shall start with a simple program with  pretty obvious errors as shown below. 
The code asks user to input amount of meat in kgs, milk in pints and units of bread bought from a store. The code then calculates the sub-total by calculating price for each product. It also, calculates amount of tax due on all these products and displays that to the user. Finally it adds up the sub-total and tax amount and print the grand total. 

Let's run this program and see if it works as expected. 



```python
meat = float(input('Enter weight in kg:'))
meatPrice = 1 * meat
meatTax = 0.04 * meatPrice

milk = float(input('Enter in pints:'))
milkPrice = 0.5 * milk 
milkTax = 0.03 * milkPrice

bread = float(input('Enter number of bread units'))
breadPrice = bread ** 0.75
breadTax = 0.02 * breadPrice

total = meatPrice+milkPrice+meatPrice
tax = meatTax+milkTax+breadTax

print ("Sub-Total:", total)
print ("Taxes:", tax)
print ("Total bill:", total+meatTax)

```

    Enter weight in kg:2
    Enter in pints:3
    Enter number of bread units4
    Sub-Total: 5.5
    Taxes: 0.18156854249492382
    Total bill: 5.58


So something is obviously wrong here. The total amount looks wrong and also the sub-total and taxes don't add up to the grand total. As program runs fine so we suspect there is something wrong with the calculations. Although you might be able to spot the errors but let's see how to do this with the `pdb`. 

### Getting started with debugging

We shall use `ipdb` for debugging our code which is a variant of `pdb` to be used in jupyter notebooks. For this we shall import `IPython.core.debugger` into our working environment at the top of the cell we want to debug. In order to break into the debugger from a running program (i.e. creating a breakpoint), we can simply insert `set_trace()` at place in the code where you want to break into the debugger. We can then step through the code following this statement, and provide extra parameters as we shall below. 

Let's first import ipdb as mentioned above and import it as `debug`. We shall insert `set_trace()` just before the calculations are made. On the ipdb prompt, a first check that we can make is to check the current values of variables we are interested in i.e. tax and sub-total. For this, we can simply print those values as `p <variable_name>`. 

We shall check the values and then press `q` to quit the debugger. We can also use `c` for continue till the program reaches the end of execution.


```python
import IPython.core.debugger as debug


meat = float(input('Enter weight in kg:'))
meatPrice = 1 * meat
meatTax = 0.04 * meatPrice

milk = float(input('Enter in pints:'))
milkPrice = 0.5 * milk 
milkTax = 0.03 * milkPrice

bread = float(input('Enter number of bread units'))
breadPrice = bread ** 0.75
breadTax = 0.02 * breadPrice

# Create a break point
debug.set_trace()

total = meatPrice+milkPrice+meatPrice
tax = meatTax+milkTax+breadTax

print ("Sub-Total:", total)
print ("Taxes:", tax)
print ("Total bill:", total+meatTax)

```

    Enter weight in kg:2
    Enter in pints:3
    Enter number of bread units4
    --Return--
    None
    > [0;32m<ipython-input-30-fb8a670fe386>[0m(17)[0;36m<module>[0;34m()[0m
    [0;32m     15 [0;31m[0;34m[0m[0m
    [0m[0;32m     16 [0;31m[0;31m# Create a break point[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m---> 17 [0;31m[0mdebug[0m[0;34m.[0m[0mset_trace[0m[0;34m([0m[0;34m)[0m[0;34m[0m[0m
    [0m[0;32m     18 [0;31m[0;34m[0m[0m
    [0m[0;32m     19 [0;31m[0mtotal[0m [0;34m=[0m [0mmeatPrice[0m[0;34m+[0m[0mmilkPrice[0m[0;34m+[0m[0mmeatPrice[0m[0;34m[0m[0m
    [0m
    ipdb> c
    Sub-Total: 5.5
    Taxes: 0.18156854249492382
    Total bill: 5.58


So you see how we can create a breakpoint to step into the debugger and check variable values at that point in the program. Looking at above, its still hard to work out where the problem is.

Let's explore what other features our debugger offers to dig deeper into the problem. Here is a list of basic commands that will come in handy. 

* **l(ist)**:  Displays 11 lines around the current line or continue the previous listing.
* **s(tep)**:  Execute the current line, stop at the first possible occasion (you would normally step through individual lines of code).
* **b(reak)**: Set a breakpoint (depending on the argument provided).


* **w(here)**:  Display the file and line number of the current line
* **n(ext)**: execute the current line
c(ontinue) execute until a breakpoint is encountered
r(eturn) execute until the current function’s return is
encountered

Let's run the program again and try above commands as follows: 

* Move `set_trace()` to an earlier location to step into debugger earlier.
* Use `b <line_number>` to create breakpoints where price and tax is calculated. 
* Use `c` to continue to next break point. 
* use `p <variable_name>` to check values of variables.
* Repeat above two steps till the end of execution. 



```python
import IPython.core.debugger as debug

# Create a break point
debug.set_trace()

meat = float(input('Enter weight in kg:'))
meatPrice = 1 * meat
meatTax = 0.04 * meatPrice

milk = float(input('Enter in pints:'))
milkPrice = 0.5 * milk 
milkTax = 0.03 * milkPrice

bread = float(input('Enter number of bread units'))
breadPrice = 0.75 ** bread
breadTax = 0.02 * breadPrice

total = meatPrice+milkPrice+meatPrice
tax = meatTax+milkTax+breadTax

print ("Sub-Total:", total)
print ("Taxes:", tax)
print ("Total bill:", total+meatTax)

```

    --Return--
    None
    > [0;32m<ipython-input-4-19a874eb943b>[0m(4)[0;36m<module>[0;34m()[0m
    [0;32m      2 [0;31m[0;34m[0m[0m
    [0m[0;32m      3 [0;31m[0;31m# Create a break point[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m----> 4 [0;31m[0mdebug[0m[0;34m.[0m[0mset_trace[0m[0;34m([0m[0;34m)[0m[0;34m[0m[0m
    [0m[0;32m      5 [0;31m[0;34m[0m[0m
    [0m[0;32m      6 [0;31m[0mmeat[0m [0;34m=[0m [0mfloat[0m[0;34m([0m[0minput[0m[0;34m([0m[0;34m'Enter weight in kg:'[0m[0;34m)[0m[0;34m)[0m[0;34m[0m[0m
    [0m
    ipdb> b 18
    Breakpoint 4 at <ipython-input-4-19a874eb943b>:18
    ipdb> b 19
    Breakpoint 5 at <ipython-input-4-19a874eb943b>:19
    ipdb> c
    Enter weight in kg:2
    Enter in pints:3
    Enter number of bread units4
    None
    > [0;32m<ipython-input-4-19a874eb943b>[0m(18)[0;36m<module>[0;34m()[0m
    [0;32m     16 [0;31m[0mbreadTax[0m [0;34m=[0m [0;36m0.02[0m [0;34m*[0m [0mbreadPrice[0m[0;34m[0m[0m
    [0m[0;32m     17 [0;31m[0;34m[0m[0m
    [0m[1;31m4[0;32m--> 18 [0;31m[0mtotal[0m [0;34m=[0m [0mmeatPrice[0m[0;34m+[0m[0mmilkPrice[0m[0;34m+[0m[0mmeatPrice[0m[0;34m[0m[0m
    [0m[1;31m5[0;32m    19 [0;31m[0mtax[0m [0;34m=[0m [0mmeatTax[0m[0;34m+[0m[0mmilkTax[0m[0;34m+[0m[0mbreadTax[0m[0;34m[0m[0m
    [0m[0;32m     20 [0;31m[0;34m[0m[0m
    [0m
    ipdb> p meatPrice
    2.0
    ipdb> p milkPrice
    1.5
    ipdb> p breadPrice
    0.31640625
    ipdb> c
    None
    > [0;32m<ipython-input-4-19a874eb943b>[0m(19)[0;36m<module>[0;34m()[0m
    [0;32m     17 [0;31m[0;34m[0m[0m
    [0m[1;31m4[0;32m    18 [0;31m[0mtotal[0m [0;34m=[0m [0mmeatPrice[0m[0;34m+[0m[0mmilkPrice[0m[0;34m+[0m[0mmeatPrice[0m[0;34m[0m[0m
    [0m[1;31m5[0;32m--> 19 [0;31m[0mtax[0m [0;34m=[0m [0mmeatTax[0m[0;34m+[0m[0mmilkTax[0m[0;34m+[0m[0mbreadTax[0m[0;34m[0m[0m
    [0m[0;32m     20 [0;31m[0;34m[0m[0m
    [0m[0;32m     21 [0;31m[0mprint[0m [0;34m([0m[0;34m"Sub-Total:"[0m[0;34m,[0m [0mtotal[0m[0;34m)[0m[0;34m[0m[0m
    [0m
    ipdb> p tax
    0.18156854249492382
    ipdb> p total
    5.5
    ipdb> c
    Sub-Total: 5.5
    Taxes: 0.131328125
    Total bill: 5.58


Okay so looking at above, we can see that bread price looks wrong at first break point. Also, the total does not reflect total prices as seen at first break point. 

Lets see how bread price is being calculated

```python 
breadPrice = 0.75 ** bread

```
And there is our first error. a double `**` is used in python for taking power. 

Also, let's see where total is being calculated
```python
total = meatPrice+milkPrice+meatPrice
```

Here we can see another error as `meatPrice` is being added twice and there is no `breadPrice`. 

Let's fix these two errors and run the code again. Now we shall set one breakpoint where all the calculations have been made.


```python
import IPython.core.debugger as debug

# Create a break point
debug.set_trace()

meat = float(input('Enter weight in kg:'))
meatPrice = 1 * meat
meatTax = 0.04 * meatPrice

milk = float(input('Enter in pints:'))
milkPrice = 0.5 * milk 
milkTax = 0.03 * milkPrice

bread = float(input('Enter number of bread units'))
breadPrice = 0.75 * bread
breadTax = 0.02 * breadPrice

total = meatPrice+milkPrice+breadPrice
tax = meatTax+milkTax+breadTax

print ("Sub-Total:", total)
print ("Taxes:", tax)
print ("Total bill:", total+meatTax)

```

    --Return--
    None
    > [0;32m<ipython-input-1-b5346ce2703b>[0m(4)[0;36m<module>[0;34m()[0m
    [0;32m      2 [0;31m[0;34m[0m[0m
    [0m[0;32m      3 [0;31m[0;31m# Create a break point[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m----> 4 [0;31m[0mdebug[0m[0;34m.[0m[0mset_trace[0m[0;34m([0m[0;34m)[0m[0;34m[0m[0m
    [0m[0;32m      5 [0;31m[0;34m[0m[0m
    [0m[0;32m      6 [0;31m[0mmeat[0m [0;34m=[0m [0mfloat[0m[0;34m([0m[0minput[0m[0;34m([0m[0;34m'Enter weight in kg:'[0m[0;34m)[0m[0;34m)[0m[0;34m[0m[0m
    [0m
    ipdb> b 19
    Breakpoint 1 at <ipython-input-1-b5346ce2703b>:19
    ipdb> c
    Enter weight in kg:2
    Enter in pints:3
    Enter number of bread units4
    None
    > [0;32m<ipython-input-1-b5346ce2703b>[0m(19)[0;36m<module>[0;34m()[0m
    [0;32m     17 [0;31m[0;34m[0m[0m
    [0m[0;32m     18 [0;31m[0mtotal[0m [0;34m=[0m [0mmeatPrice[0m[0;34m+[0m[0mmilkPrice[0m[0;34m+[0m[0mbreadPrice[0m[0;34m[0m[0m
    [0m[1;31m1[0;32m--> 19 [0;31m[0mtax[0m [0;34m=[0m [0mmeatTax[0m[0;34m+[0m[0mmilkTax[0m[0;34m+[0m[0mbreadTax[0m[0;34m[0m[0m
    [0m[0;32m     20 [0;31m[0;34m[0m[0m
    [0m[0;32m     21 [0;31m[0mprint[0m [0;34m([0m[0;34m"Sub-Total:"[0m[0;34m,[0m [0mtotal[0m[0;34m)[0m[0;34m[0m[0m
    [0m
    ipdb> p meatPrice
    2.0
    ipdb> p milkPrice
    1.5
    ipdb> p breadPrice
    3.0
    ipdb> p total
    6.5
    ipdb> p meatTax
    0.08
    ipdb> p milkTax
    0.045
    ipdb> p breadTax
    0.06
    ipdb> p tax
    *** NameError: name 'tax' is not defined
    ipdb> p milkTax+meatTax+breadTax
    0.185
    ipdb> c
    Sub-Total: 6.5
    Taxes: 0.185
    Total bill: 6.58


Okie now, everything is fine till the grand total as it does not reflect total+tax amount. 

Let's check the point where grand total is being calculated. Here is the line:

```python
print ("Total bill:", total+meatTax)

```

So we can see that we are adding `meatTax` to the total and instead of `tax`. Let's fix and run again.


```python
import IPython.core.debugger as debug

# Create a break point
debug.set_trace()

meat = float(input('Enter weight in kg:'))
meatPrice = 1 * meat
meatTax = 0.04 * meatPrice

milk = float(input('Enter in pints:'))
milkPrice = 0.5 * milk 
milkTax = 0.03 * milkPrice

bread = float(input('Enter number of bread units'))
breadPrice = 0.75 * bread
breadTax = 0.02 * breadPrice

total = meatPrice+milkPrice+breadPrice
tax = meatTax+milkTax+breadTax

print ("Sub-Total:", total)
print ("Taxes:", tax)
print ("Total bill:", total+tax)

```

    --Return--
    None
    > [0;32m<ipython-input-2-c57e97346986>[0m(4)[0;36m<module>[0;34m()[0m
    [0;32m      2 [0;31m[0;34m[0m[0m
    [0m[0;32m      3 [0;31m[0;31m# Create a break point[0m[0;34m[0m[0;34m[0m[0m
    [0m[0;32m----> 4 [0;31m[0mdebug[0m[0;34m.[0m[0mset_trace[0m[0;34m([0m[0;34m)[0m[0;34m[0m[0m
    [0m[0;32m      5 [0;31m[0;34m[0m[0m
    [0m[0;32m      6 [0;31m[0mmeat[0m [0;34m=[0m [0mfloat[0m[0;34m([0m[0minput[0m[0;34m([0m[0;34m'Enter weight in kg:'[0m[0;34m)[0m[0;34m)[0m[0;34m[0m[0m
    [0m
    ipdb> c
    Enter weight in kg:2
    Enter in pints:3
    Enter number of bread units4
    Sub-Total: 6.5
    Taxes: 0.185
    Total bill: 6.685


Perfect. Everything makes sense now. 

Now you might be thinking why do we need to go through all this trouble to identify errors which could have been spotted easily otherwise ! 

In the real world situations, you might be incolved in projects where a software may contain upto millions of lines of code and following a really complicated program logic. Finding errors manually in such situations could be time-consuming and inefficient activity. Being able to step into the debugging mode, create break points, checking variable state at checkpoints - as we saw in this lesson, could be useful towards debugging a large software with much ease. 

Here is a quick infographic that you can use along side debugging techniques to identify possible places for errors in your code. `pdb` in python is a detailed debugger and offers lots of other commands not mentioned in this lesson. You are strong advised to visit the [official `pdb` documentation at this location](https://docs.python.org/2/library/pdb.html) to learn more about pdb.

![](https://mdalums95.files.wordpress.com/2013/12/wrujv6r.png)

### Summary

In this lesson we learned to debug python code using `ipdb`. We saw why debugging is considered an important skill for coders and how manually identifying errors for large softwares is not a practical solution. We learnt basic commands in python debugger to identify and eliminate errors in a a simple code. You are advised to use these skills in your projects.
