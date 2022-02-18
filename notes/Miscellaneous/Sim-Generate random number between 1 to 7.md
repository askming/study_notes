# Generate random number 1 to 7 using a fair dice

## Method 1:
  - There are a bunch of options.  Here's an easy one:  roll the die twice, keeping track of the first and second roll.  There are 36 outcomes:

      (1,1), (1,2), (1,3), ..., (6,6)

    (The first number is what you got on the first roll; the second is what you got on the second.)

  - If you get a (6,6), just re-roll the die twice again until you get a non-(6,6).

  - Now there are 35 equally-likely outcomes, so divide them into 7 groups of 5 corresponding to the 7 choices among which you want to choose.

  - Here's an easy calculation that will do it.  Suppose you roll a (x, y) with x = number on first roll and y = number on second roll.  First calculate the following number:

 $$ N = 6(x-1) + y.$$

## Method 2:
Python code:

  ```Python
  import random 
  # import this to use the random feature */

  dicerolls = [ [] for i in range(7) ]
  for i in range(len(dicerolls)):
   dicerolls[i] = [ int(random.random()*6) for number in range(500)]

  # print out winners and identify my random number from 1:7
  winner = [sum(i) for i in dicerolls ]
  print winner
  print max(winner), "- random number:", winner.index(max(winner))+1
  ```
  - How it works: Roll N*7 times. Each result gets put into one of 7 lists, like so (when N = 13):

    1: [1,3,3,1,4,1,4,3,4,5,5,6,6]

    2: [5,3,4,4,2,3,4,2,3,3,2,4,3]

    3: [5,2,4,6,2,3,4,2,3,2,4,5,4]

    …

    7: [2,4,2,3,4,5,1,6,2,4,6,2,4]

    Because each individual roll is independent and random, it doesn’t matter how many times you roll, as long as each list is given the same amount of rolls. What’s important is that there are a sufficient number of rolls to prevent ties (in the code, there are 2^7). After N rolls, you can sum the contents of the list, and the largest position is the outcome of your randomly generated number.

