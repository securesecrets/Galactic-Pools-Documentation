# Prize Distribution Algorithm

PoolParty uses an algorithm that can offer a virtually infinite number of prizes to be distributed.

### Overview

Prizes are distributed using weekly draws

* Each draw has a set of prize distribution parameters, including a randomly generated winning sequence.
* When a user claims rewards, a random sequence is generated and matched with the winning sequence.
* The user only gets to generate a limited number of random sequences. Since one random sequence can only be generated if one token worth of liquidity was provided during the week, this limit parameter can be updated according to the community's will. e.g., In the future, The user can generate 2 random sequences by providing one token worth of liquidity.&#x20;
* The tier to which a random sequence matches the draw's winning number determines the prize size for the pick. The matching algorithm is configured per draw.

#### Matching Algorithm

Each time a random sequence is generated, it's compared to the winning sequence. The sequence is broken down into an array of single numbers.

The winning sequence and random sequence are 6 digits sequences by default. &#x20;

Let's see an example.

<table><thead><tr><th align="center">index 0</th><th data-type="number">Index 1</th><th data-type="number">Index 2</th><th data-type="number">Index 3</th><th data-type="number">Index 4</th><th data-type="number">Index 5</th></tr></thead><tbody><tr><td align="center">1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td></tr><tr><td align="center">1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td></tr></tbody></table>

\
\


