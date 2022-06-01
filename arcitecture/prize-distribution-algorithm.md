# Prize Distribution Algorithm

PoolParty uses an algorithm that can offer a virtually infinite number of prizes to be distributed.

### Overview

Prizes are distributed using weekly draws

* Each draw has a set of prize distribution parameters, including a randomly generated winning sequence.
* When a user claims rewards, a random sequence is generated and matched with the winning sequence.
* The tier to which a random sequence matches the draw's winning number determines the prize size for the pick. The matching algorithm is configured per draw.

#### Matching Algorithm

Each time a random sequence is generated, it's compared to the winning sequence. The sequence is broken down into an array of single numbers.

The winning sequence and random sequence are 6 number sequences by default.

Let's stack them up side-by-side.

| index 0 | index 1 | index 2 | index 3 | index 4 | index 5 |
| ------- | ------- | ------- | ------- | ------- | ------- |
| 1       | 2       | 3       | 4       | 5       | 6       |
| 1       | 2       | 3       | 9       | 8       | 6       |

The "tier" of the match is _the cardinality less the first N matching numbers_.

In the above example, the first three numbers match:

As it can be said, the match's tier is 6-3 or three.

The tier of the match is used to determine the prize ter of the winner. Tier 0 is the grand prize; Tier 1 is in second place.

#### Total Random Generated sequences.

Each member gets one chance for every X amount of liquidity provided during the staking period to generate a random number and match it with the winning sequence. E.g., User A deposits 100 SCRT at the start of the period. When claiming rewards, he'll get 100 chances to try his luck at winning prizes. We compute the user's historic liquidity contribution using their Time-Weightage Average.

#### Generating Random numbers.

For each random sequence, six random numbers are generated. Within a range of 0 and 9.&#x20;

E.g: 0-2-4-5-8-1.

**But this range will increase as the number of users increase so that not all the users draw the same sequence. Theoretically this range could go to infinity.**

In the future, it could look like 10-12-14-15-28-11.

#### Calculating a prize

A user can determine the value of a winning pick based on the tier of the match, the prize distribution, and the total prize. The prize distribution has an array of fractions that determine the prize portion allocated to each tier.

For example

| Tier | Percentage | Number of winners | Prize per winner |
| :--: | :--------: | :---------------: | :--------------: |
|   0  |     20%    |         1         |      2000.0      |
|   1  |     10%    |         3         |       333.3      |
|   2  |     14%    |         9         |       155.6      |
|   3  |     12%    |         27        |       44.4       |
|   4  |     19%    |         81        |       23.5       |
|   5  |     25%    |        243        |       10.2       |

#### Claiming Prizes

It is advised for users to claim their rewards as fast as possible. Since the algorithm works on a first-come, first-serve basis, once a particular prize is claimed, it cannot be claimed again. E.g., Let's say the winning sequence is 1-2-3-4-5-6, so User A can match the winning sequence and wins the Tier 0 prize. After some time, if User B produces the same sequence, he won't be able to win Tier 0 prize, but he's eligible to win Tier 1 to Tier 5 prizes if all those prizes are not claimed yet.

#### The limited sequence generated per user

The user only gets to generate a limited number of random sequences. Since one random sequence can only be generated if one token worth of liquidity was provided during the week, this limit parameter can be updated according to the community's will. e.g., In the future, The user can generate two random sequences by providing one token worth of liquidity.&#x20;
