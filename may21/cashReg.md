<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Challenge</h1>

Create a function that returns the correct change to the customer, itemised by note/coin. The function takes in three arguments: The **price** to pay, the cash **paid** by the customer and the **till float** array.

If there is not enough float in the till return the following object with an empty change array:

```js
{status: 'INSUFFICIENT_FUNDS', change: []}

```

<br>

If the float equals the change due return the change array and close the till:

```js
{status: 'Closed', change: change}

```

<br>

If there is enough float but not able to return the correct change due to the note/coin type being larger than change due and no smaller notes/coins available that can make up the change due:

```js
{status: 'INSUFFICIENT_FUNDS', change: []}

```

<br>

Lastly if there is enough in the float and the correct change can be returned to the customer, return the `finalArr` array which is the change paid itemised by note/coin type and value:

```js
{status: 'OPEN', change: finalArr}

```

<br>

The second two outcomes are the tricky ones to work with, focusing on the 4th outcome: The first level of complexity is working with the totals, the second level is having to itemise by each note/coin and tracking the double entry for each type within the till (`cashTill`) and change paid (`finalArr`) and tracking the `changeDue` left to pay at every stage.

I am a certified bookkeeper but honestly I slept on this one for two nights.....But I got there and here is how I broke it down.
<br><br>

<h1 style="color: rgb(100, 100, 100); font-weight: 500;">Solution</h1>

I have broken the below function down into 5 parts, lets get into it.

The `checkCashRegister` function takes three arguments with `cid` standing for `cash in draw` which moving forward we will call float and is managed via the `cashTill` array. Each note/coin type within the `cashTill` array is a separate sub-array with index[0] being the type name and index[1] being the value of funds within the float for that coin/note.

Jumping into the function, the const `values` is an array of objects each of which provides the unit value for each note/coin (US currency). We will come back to this as we progress.

Next we want to create the `cashTill` array ordered by biggest to smallest, again this will become clear shortly.

Then `changeDue` is created which is used to track the current value of change still to pay, as each double entry is posted.

Lastly in this first section we deal with two of the potential outcomes via if statements and get them out of the way. If either of these conditions is met the respective response is returned and the execution thread returns to the global context.

<img src="https://user-images.githubusercontent.com/73107656/117597179-fc101a80-b13c-11eb-8a7e-1ea0d96cea94.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >
<br><br>

If the `totalAvailable` within the float is more than `changeDue` we skip the first two if statements and continue down.

This next section begins to deal with allocating change given back to the customer by coin/note. and either adding a new note type to `finalArr` or if already exists adding to the current value.

A variable `tempArr` is defined, which we will come back to as we move down and a const `finalArr` which to recap is the final array of change given back to the customer itemised by coin/note type.

A const `names` is defined and assigned an empty array, but is used to mirror the note/coin types within `finalArr`. This is used to prevent duplicates of the same type being added and rather adding to an existing type instead.

Lastly in this section the function `converter` is defined and we can see the first if statement within this function:

If `changeDue` is more than 0 dive in. We loop through the `values` array and when we come to a coin/note that has a unit value that is less than or equal to `changeDue` we enter the next if statement that loops through `cashTill` to check if the coin/note type is available in the float and if there is enough funds of that type.

If there is, we push one unit of that type onto `tempArr`. Notice that before we push, we also check that `tempArr` has a length of 0. That is because we only want the first item each time `converter` is invoked.

So for example lets say changeDue is £2, we have just added £1 to tempArr and to get the other £1 we invoke `converter` again. We will deal with this logic next. So far we have identified the first coin/note to be repaid as change and added it to the `tempArr` array.

Lets move on.

<img src="https://user-images.githubusercontent.com/73107656/117597196-00d4ce80-b13d-11eb-948a-37cbd591378c.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >
<br><br>

Still within the `converter` function we check if `tempArr` has a length and if so assign `tempArr[0]` to `array` which now makes it easy to access either `array[0]` **type** or `array[1]` **value**

Below that there are two main if statements, one to run if the **type** already exists in `finalArr` and the other if not.

If array[0] type exists in `finalArr` we enter the first if statement:

<img src="https://user-images.githubusercontent.com/73107656/117597201-03cfbf00-b13d-11eb-9629-036002a2d5cd.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >
<br><br>

Diving into the code above where we left off, note if this is the first time running `converter` we would be adding a new note/coin type to `finalArr` so we would be skipping down to the next if statement, for now lets assume that the current note/coin we want to add to `finalArr` already exists in finalArr so we want to identify that and add to its current value. In this situation the thread enters the if statement above and we continue:

(Above) First using a **forEach** we loop through `finalArr` looking to match any existing note/coins with `array[0]`, once found the value for that coin/note is updated to include the new debit to the `finalArr` account.

We then move down and loop through `cashTill` to identify the same note/coin type and make the opposite (credit) to the till float.

Once the `finalArr` **forEach** has played out we then reconcile the `changeDue` value with the above transaction.

We then check to see if there is any change still owed to the customer and if so enter the next nested if statement, which resets `tempArr` back to an empty array and invokes `converter`. The process runs again but with new current value of `changeDue` and the updated `cashTill` and `finalArr`.

(Below) If there is still `changeDue` to the customer, and `array[0]` (coin/note type) dos not already exist in `finalArr` we jump into the if statement below.

First we add the coin/note type to `names`, then we push the coin/note, debiting `finalArr` (cash paid to the customer).

we then loop through `cashTill` to identify the coin/note type and make the corresponding credit to the till float.

Then we reconcile the `changeDue` with the above, note: use of `toFixed(2)` to calculate to two decimal places. This is essential when working with decimals via JavaScript to prevent fractional miss calculations.

If there is still change due to the customer, we enter the nested if statement, reset `tempArr` and invoke `converter` to start the process again.

<img src="https://user-images.githubusercontent.com/73107656/117597209-07fbdc80-b13d-11eb-930a-7b4c20c53484.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >
<br><br>

(Above continuation) If `changeDue` is 0 the thread does not enter any if statements within `converter` on that round and exits the function, back out into the larger `checkCashRegister` function where the `finalArr` array is returned. At that point `finalArr` is the sum total of the `changeDue` and includes an array of all the notes/coins that make up the customers change.

If all of the above if statements are false, meaning that there is still `changeDue` but the required coins/notes to pay the customer are not available so the correct change cannot be given the thread does not enter any if statements within `converter` but instead an empty array is returned.

Below is an action snap shot of how this plays out at each stage:

Cash owed is `$17.50`, cash paid is `$20.00`, the array is the current till float, lets step through the console.log sequencing:

The till starts with 2\* $1 bills and $1 of quarters.

Converter is invoked and adds a new note type ($1) to `finalArr`, `cashTill` is credited and `changeDue` is adjusted to show $1.50 to pay.

`converter` is invoked, $1 bill is identified and already exists so `finalArr` is updated to the new value of $2, `cashTill` is updated with showing that there is no more $1 bills available in the float.

`changeDue` is updated to show there is still $0.50 to pay, `converter` is invoked and the quarter coin is added to `finalArr` as a new coin type, credited from `cashTill` and `changeDue` is updated to show $0.25 still owed to the customer. The same process takes place once more except $0.25 is added to the value of the already existing quarter coin, `changeDue` equals 0 and `finalArr` is returned.

The last line in the terminal shows `finalArr`

<img src="https://user-images.githubusercontent.com/73107656/117597229-0f22ea80-b13d-11eb-8fee-2712d8c38d2d.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >
<br><br>

When I approach a problem like this again I will swap the forEach for while loops to reduce the amount of needless looping. I also found myself making everything an array, instead of working with objects, i think the added complexity funnelled me into a subconscious comfort zone of arrays. This was a real test, less fun but massively rewarding.
