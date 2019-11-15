# Why ['1' , '7', '11'].map(parseInt) returns [1, NaN, 3] in Javascript
```js
['1', '7', '11'].map(parseInt);

// => [1, NaN, 3]  🥺？？？
```

- **Radix**
`0 1 2 3 4 5 6 7 8 9 10`
Decimal counting system has a  radix (or base) of ten.
Radix is the smallest number which can be represented by more than one symbol.**Different counting systems have different radixes, and so, the same digits can refer to different numbers in counting systems**.
```
DECIMAL    BINARY    HEXADECIMAL
RADIX=10   RADIX=2   RADIX=16
0          0         0
1          1         1
2          10        2
3          11        3
.          .         .
.          .         .
.          .         .
15         1111      F
16         10000     10
17         10001     11
```

- **map()**
```js
[1, 2, 3, 4, 5].map(console.log)
// output 🤔❓
//=> 1 1 0 (5) [1, 2, 3, 4, 5]
//=> 1 2 1 (5) [1, 2, 3, 4, 5]
//=> 1 3 2 (5) [1, 2, 3, 4, 5]
//=> 1 4 3 (5) [1, 2, 3, 4, 5]
//=> 1 5 4 (5) [1, 2, 3, 4, 5]
```
The outputs above is not answers we expect. Explanation :
```js
[1, 2, 3, 4, 5].map(console.log)

//The above is equivalent to :

[1, 2, 3, 4, 5].map(
	(val, index, array) => {
		console.log(val, index, array)
	}
)
```

## Bringing it together
`parseInt()` takes two arguments: `string` and `radix`. **If the radix provided is false, then by default, radix is set to 10**

```js
parseInt('11');               => 11
parseInt('11', 2);            => 3
parseInt('11', 16);           => 17

parseInt('11', undefined);    => 11 (radix is falsy)
parseInt('11', 0);            => 11 (radix is falsy)
```

## Summary
`['1', '7', '11'].map(parseInt)` doesn't work as intended because `map` passes three arguments to `parseInt()` on each iteration. The second argument `index` is passed into `parseInt()` as a **radix**

When `index` equals: 

-  0 : 0 means false, so `parseInt('1',0)` equals `parseInt('1',10)`
-  1 : Radix can't be 1 I think 👀 
-  2 : Typical representation of binary :
   1 = 1
   2 = 10
   **3 = 11**
   4 = 100
   5 = 101
If we want get the intended result: 
```js
['1', '7', '11'].map(numStr => parseInt(numStr))
```