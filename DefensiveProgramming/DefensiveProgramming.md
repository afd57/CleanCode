# Defensive Programming

The practice of writing programs that check their own operation to catch errors as early as possible. [1]

The idea of defensive programming is based on **defensive driving**. In defensive driving, you adopt the mind-set that you are never sure what the other drivers are going to do. That way, you make sure that if they do something dangerous you won't be hurt. <u>You take the responsibility of protecting yourself even when it might be the other driver's fault.</u> [2]

Defensive programming practices are often used where [high availability](https://en.wikipedia.org/wiki/High_availability), [safety](https://en.wikipedia.org/wiki/Safety), or [security](https://en.wikipedia.org/wiki/Computer_security) is needed.[3]

<img src="http://www.pngall.com/wp-content/uploads/4/Security-Shield-PNG-File.png" alt="Defense Png & Free Defense.png Transparent Images #115086 - PNGio" style="zoom:11%;" />

Defensive programming is an approach to improve software and source code, in terms of: 

- General quality – reducing the number of [software bugs](https://en.wikipedia.org/wiki/Software_bug) and problems.
- Making the [source code](https://en.wikipedia.org/wiki/Source_code) comprehensible – the source code should be readable and understandable so it is approved in a [code audit](https://en.wikipedia.org/wiki/Code_audit).
- Making the software behave in a predictable manner despite unexpected inputs or user actions.



# Principles of Defensive Programming [4]

The few basic rules of [defensive programming](http://c2.com/cgi/wiki?DefensiveProgramming) are explained in a short chapter in Steve McConnell’s classic book on programming, [Code Complete](http://cc2e.com/):



1. Protect your code from invalid data coming from “outside”, wherever you decide “outside” is. Data from an external system or the user or a file, or any data from outside of the module/component. Establish “barricades” or “safe zones” or “trust boundaries” – everything outside of the boundary is dangerous, everything inside of the boundary is safe. In the barricade code, [validate all input data](http://programmers.stackexchange.com/questions/103471/how-should-i-handle-invalid-user-input): check all input parameters for the correct type, length, and range of values. Double check for limits and bounds.
2. After you have checked for bad data, decide how to handle it. **Defensive Programming is NOT about swallowing errors or hiding bugs.** It’s about deciding on the [trade-off between robustness (keep running if there is a problem you can deal with) and correctness (never return inaccurate results)](http://richarddingwall.name/2010/02/10/correctness-vs-robustness/). Choose a strategy to deal with bad data: return an error and stop right away (fast fail), return a neutral value, substitute data values, … Make sure that the strategy is clear and consistent.
3. Don’t assume that a function call or method call outside of your code will work as advertised. Make sure that you understand and test error handling around external APIs and libraries.
4. **Use assertions to document assumptions and to highlight “impossible” conditions, at least in development and testing.** This is especially important in large systems that have been maintained by different people over time, or in high-reliability code.
5. Add diagnostic code, logging and tracing intelligently to help explain what’s going on at run-time, especially if you run into a problem.
6. **Standardize error handling.** Decide how to handle “normal errors” or “expected errors” and warnings, and do all of this consistently.
7. Use exception handling only when you need to, and make sure that you understand the language’s exception handler inside out.

# Practices

There is no exact way to convert your code into a defensive code. But this rules make easier developing defensive code. 

- Write specifications for functions
- Modularize programs
-  Check conditions on inputs/outputs (assertions)

Also firstly developer should find unexpected cases for the code.

## Exceptions

- What happens when procedure execution hits an unexpected condition?
  - Exception Handling

- What to do when encounter an error?
  - fail silently: substitute default values or just continue. **bad idea!** user gets no warning.
  - return an “error” value. Don’t return special values when an error occurred and then check whether ‘error value’ was returned
  - stop execution, signal error condition. in Python: raise an exception
  - If problem can be resolve, try it again. (HTTP request etc.)



```python
if __name__ == "__main__":
   ratios = []
   L1 = input().split(',')
   L2 = input().split(',')
   for index in range(len(L1)):
       ratios.append(L1[index]/L2[index])
   print(ratios)
```

This code is open to failure. 



Covert to defensive code.

```python
def get_ratios(L1, L2):
    """
    Assumes: L1 and L2 are lists of equal length of numbers
    Returns: 	a list containing L1[i]/L2[i]
    """
    ratios = []
    for index in range(len(L1)):
        try:
            ratios.append(L1[index]/L2[index])
        except ZeroDivisionError:
            ratios.append(float('nan')) #nan = not a number
        except:
            raise ValueError('get_ratios called with bad arg')
    return ratios
```

- separate except clauses to deal with a particular type of exception



## Assertions

I want to be sure that assumptions on state of computation are as expected. use an assert statement to raise an AssertionError exception if assumptions not met. Assertion is good example for defensive programming.

- assertions don’t allow a programmer to control response to unexpected conditions
- typically used to check inputs to functions, but can be used anywhere
- can be used to check outputs of a function to avoid propagating bad values
- can make it easier to locate a source of a bug



### WHERE TO USE ASSERTIONS?

- use assertions to
  • check types of arguments or values
  • check that invariants on data structures are met
  • check constraints on return values
  • check for violations of constraints on procedure (e.g. no
  duplicates in a list)

### Example 

```python
def avg(grades):
    return sum(grades)/len(grades)
```

Defensive

```python
def avg(grades):
    assert len(grades) != 0, 'no grades data'
    return sum(grades)/len(grades)
```





# References

[1] https://swcarpentry.github.io/python-novice-inflammation/reference/#defensive-programming

[2] http://www.philadelphia.edu.jo/academics/shanna/uploads/Defensive_Programming.doc

[3] https://en.wikipedia.org/wiki/Defensive_programming

[4] https://dzone.com/articles/defensive-programming-just

[5] https://swcarpentry.github.io/python-novice-inflammation/10-defensive/index.html

[6] https://mitocw.ups.edu.ec/courses/electrical-engineering-and-computer-science/6-0001-introduction-to-computer-science-and-programming-in-python-fall-2016/lecture-slides-code/MIT6_0001F16_Lec7.pdf
