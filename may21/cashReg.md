<h1 style="color: rgb(100, 100, 100); font-weight: 500;">The Challenge</h1>

Create a function that returns the correct change to the customer, itemised by note/coin. the function takes in three arguments: The **price** to pay, the cash **paid** by the customer and the **till float** array.

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

<img src="https://user-images.githubusercontent.com/73107656/117597209-07fbdc80-b13d-11eb-930a-7b4c20c53484.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >
<br><br>

<img src="https://user-images.githubusercontent.com/73107656/117597229-0f22ea80-b13d-11eb-8fee-2712d8c38d2d.png" alt="ProDev" width="100%"  style="border-radius: 10px;" >
<br><br>
