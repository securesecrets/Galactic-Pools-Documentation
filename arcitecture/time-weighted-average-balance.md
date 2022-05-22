# Time-Weighted Average Balance

We may determine a user's balance at any moment in the past or their average balance held between two occasions by tracking a Time-Weighted Average Balance. We may calculate their liquidity share for that period using their average balance and total supply.

This helps us calculate the user's exact contribution to the pool.

For example:

* if a user held 100 tokens between March 3 and March 10, their average balance was 100.
* If they bought another 100 tokens halfway through, their average between March 3 and March 10 would be 150.

## Computing the TWAB

Each TWAB record is a tuple of an amount and timestamp. The amount stores the cumulative time-weighted balance, and the timestamp is the time at which the twab was recorded. Immediately before a user's balance changes, we record a new TWAB.

The formula is:

```python
new TWAB amount = last TWAB amount + current balance * (current time - last TWAB timestamp)
new TWAB timestamp = current time
```

### Example[​](https://dev.pooltogether.com/protocol/introduction/#example) <a href="#example" id="example"></a>

Let's assume a user's transfer history looks like so:

| time | action  | amount |
| ---- | ------- | ------ |
| 0    | receive | 100    |
| 10   | receive | 50     |
| 20   | send    | 100    |
| 30   | send    | 20     |

Let's go through it step-by-step.

The first twab would be:

```javascript
amount = 0 + 0 * (0 - 0) = 0
timestamp = current time = 0
```

The second twab would be:

```javascript
amount = 0 + 100 * (10 - 0) = 1000
timestamp = current time = 10
```

The third twab would be:

```javascript
amount = 1000 + 150 * (20 - 10) = 2500
timestamp = current time = 2
```

The fourth twab would be:

```python
amount = 2500 + 50 * (30 - 20) = 3000
timestamp = current time = 30
```

And so on. Our twab table would look like this:

| time | twab amount |
| ---- | ----------- |
| 0    | 0           |
| 10   | 1000        |
| 20   | 2500        |
| 30   | 3000        |

Now, let's ask the question:

> What was the user's average balance between t = 0 and t = 20?

We can calculate the average by calculating the difference in the cumulative amount and dividing by the elapsed time:

```python
average balance = (last TWAB amount - first TWAB amount) / (last TWAB timestamp - first TWAB timestamp)
```

So we'd have

```python
average balance = (2500 - 0) / (20 - 0) = 125
```

Does this intuitively feel correct? The user held 100 for 10 seconds, then held 150 for ten seconds. Yup. That's right.

## Measuring Liquidity Contribution

We can use a TWAB for the user's balance and a TWAB for the total supply to determine a user's share of the liquidity supplied in the past.

The formula is quite simple:

```python
user's fraction of liquidity = average balance between (t1, t2) / average total supply between (t1, t2)
```
