<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Challenge</h1>

Take the passed in string, decode it and return the decoded string. The string is encoded using the Caesar Cipher `ROT13` method.  The Rot 13 cipher uses the letter 13 places before the decoded letter. 

<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Solution</h1>

I decided to map a decoding object named `key`, then to turn the passed in string into an array of letters using the spread operator. Then map through the array of letters matching each one using the the `key` and returning the output letter to a new array. Lastly turn the output array back into a string and return.

A really fun one to both research and code.

Each letter **key** in the below `key` object has a letter **value** 13 places forward. The alphabet goes round in a loop. 

<img src="https://user-images.githubusercontent.com/73107656/117320423-1ff50700-ae84-11eb-8ddb-c7b23c1da215.png" alt="header image" width="100%"  style="border-radius: 10px; margin: 30px auto" >