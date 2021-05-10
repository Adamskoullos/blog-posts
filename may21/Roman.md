<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Challenge</h1>

Create a function that takes in a number and converts it to the respective Roman numeral.

Only standard Roman numerals are used and no number can have more than three Roman numeral characters. What do these rules mean?, well after some research turns out that standard Roman numerals go up to 3,999. Numbers above this level require special characters above the numeral. So the solution only has to deal with an input up to 3,999.

The second rule, prevents chunking and forces the solution to handle each decimal number, which is the whole point and where the fun of the challenge is.

Once I understood this much I went to work to figure out the main decimal building blocks and their corresponding numerals.

<h1 style="color: rgb(100, 100, 100); font-weight: 500;">Solution</h1>

Once I had mapped the core numeral building blocks I knew I had to work though them from biggest to smallest reducing the passed in value each time...I wrestled with this for ages. In the end I worked out a recursive function that did the job.

First up here is the visual, below that we will dive into the code and break it down:

<img src="https://user-images.githubusercontent.com/73107656/117521107-f29f7a80-afa3-11eb-9081-834650a31dcd.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >
<br><br>

Within the `convertToRoman` function definition the first const `buildingBlocks` is an array of the minimum core decimal numbers required to make up any Roman numeral needed.

Below that the `numArr` and `tempArr` are used within the `createNumArr` function which we will come back to.

Then `romanNum` will eventually be the final array of numerals before they are joined into a string

Ok so the first job using the `createNumArr` function is to create an array of roman numeral numbers that make up `num`. `num` is passed in and if `num` is greater than 0, run the code.

We loop through `buildingBlocks` biggest to smallest and when `decNum` is equal to or less than `num` it is added to `tempArr`. Now we only want the first item, so when the loop is finished, we take `tempArr[0]` and push it onto `numArr`, reduce num by the value of `tempArr[0]`, reset tempArr to an empty array and call `createNumArr` again passing in the current value of `num`

This keeps going round until `num` is 0 at which point `numArr` is an array of numbers that have a sum total of num. Each individual number now corresponds to a Roman numeral.

Ok so at this point the hard bit is done, we now loop through `numArr` matching each number and pushing the corresponding Roman numeral to the `romanNum` array.

Finally we join and return `romanNum`
