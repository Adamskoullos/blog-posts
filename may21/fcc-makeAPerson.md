<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Challenge</h1>

Create a constructor function that makes a new person and has exactly six keys. Must be able to **get** the `full name`, `first name` and `last name` and **set** the `full name`, `first name` and `last name`. At a glance we can see that we have **three setters** and **three getters** but we also need to store the current state for each variable.

With the rule of only having six keys we are being forced to utilise closure. 

<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Solution</h1>

A person is created by passing in the first and second name as a string. Diving into the constructor below, the passed in `firstAndLast` name is immediately saved to the variable `fullName`. we then split the string to assign the first and second names to their respective variables.

Then we jump down to the **getters**, that use the `this` keyword so are accessible via dot notation, which **return** the current value for the respective variable. 

<img src="https://user-images.githubusercontent.com/73107656/117306709-d3a3ca00-ae77-11eb-9690-a1a14b6e6e58.png" alt="ProDev" width="100%"  style="border-radius: 10px; margin: 30px auto" >

Moving on to the **setters**, notice they all take an argument to update the respective variable value. Thy do not return a value, they alter the private variable.

At the bottom a person `bob` is created and in the terminal we can see how the private data is only accessible via the getters and setters.  When we try to access these variables directly `undefined` is returned.

Closure can be used to reduce namespace clutter, save sensitive data that we want to keep private and to reduce the risk of data being altered directly or indirectly.