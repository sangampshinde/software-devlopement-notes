# DSA #1 — Time Complexity & Big O

- How does the amount of work grow when the input gets bigger? `That is time complexity`.

# 1. What is an algorithm?

- An algorithm is simply a step-by-step procedure to solve a problem.

Example:

```
function findMax(nums: number[]): number {
    let max = nums[0];

    for (const num of nums) {
        if (num > max) {
            max = num;
        }
    }

    return max;
}

```

The algorithm is:

```
Start with first number
       ↓
Look at every number
       ↓
Compare with current maximum
       ↓
Update maximum if necessary
       ↓
Return maximum

```
Now the DSA question is:

If the array has 10 elements, how much work?

Approximately 10 comparisons.

What about 1,000 elements?

Approximately 1,000 comparisons.

What about 1,000,000?

Approximately 1,000,000.

The work grows with n.

Therefore:

```
O(n)

```

2. What is `n`?

- `n` generally represents the size of the input.

```
const nums = [10, 20, 30, 40, 50];

```

we have:

```

n = 5

```

For:

```
const nums = [1, 2, 3, ..., 100000];

```

we have:

```
n = 100000

```

We don't care about the exact number.
We care about how the work grows.


3. `O(1)` — Constant Time

Example:

```

function getFirst(nums: number[]): number {
    return nums[0];
}

```

Whether the array contains:

```
10 elements

```
or:

```
10,000,000 elements

```
we perform essentially one operation.

```
O(1)

```


```

Input grows
    ↓
Work stays approximately the same
    ↓
O(1)

```

```

function getLast(nums: number[]): number {
    return nums[nums.length - 1];
}

```
```

O(1)

```

4. `O(n)` — Linear Time

Example:

```

function printAll(nums: number[]): void {
    for (const num of nums) {
        console.log(num);
    }
}

```

If:

```
n = 10

```

roughly 10 iterations.

If:

```
n = 1000

```

roughly 1000 iterations.

Therefore:

```
O(n)

```

```

n       work

10      10
100     100
1000    1000
10000   10000

```

The work grows directly with input size.