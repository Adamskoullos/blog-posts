<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Challenge</h1>

Extract all items from a multi layered array and return a single layered array with all items.

<h1 style="color: rgb(100, 100, 100); font-weight: 500;">Solution</h1>

This challenge includes recursion and once conquered is fairly simple but there is an interesting nuance worth noting. First up here is the solution:

<img src="https://user-images.githubusercontent.com/73107656/116835859-32d5b600-abbc-11eb-852c-9825a84d85a6.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >

This problem tripped me up for a while and forced me to think outside the box to overcome my issue. Initially I defined `finalArr` as an empty array inside the `steamrollArray` at the top...habit I guess. 

I also was not passing `finalArr` into `steamrollArray` inside the if statement. This was causing finalArr to be reset each time `steamrollArray` was invoked. 

I could have defined `finalArr` outside of `steamrollArray` but not for this challenge. In the end finalArr is initially defined when the file loads and `steamrollArray` is defined because `steamrollArray` is defined with the empty array `finalArr` as the second argument.

Then each time `steamrollArray` is invoked the current value of `finalArr` is passed in as the second argument and added to.

This reminds me of the traditional namespace pattern when the object to be added to is passed in (if it exists).

A fun one to get more familiar working with recursion.