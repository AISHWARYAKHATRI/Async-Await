# Async-Await

## How were promises handled before async and await ?

```
const p = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Promise resolved"), 10000);
});

function getData2() {
  p.then((res) => console.log(res));
  console.log("Js here does not wait for the promise to be resolved.");
}
```
- When resolving our promises like this JS engine will not waiting for us.
- It goes to the next line first, "Js here does not wait for the promise to be resolved." printed then the promise was resolved after 10 seconds and the value is logged  

## How aysnc and await handles Promises?

```
const p = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Promise resolved"), 10000);
});

async function getData() {
  const data = await p;
  console.log(data);
  console.log("This is logged after the promise has been resolved");
}
```
- The js engine in case of async await waits for the line 26 to get resolved
- Nothing is printed onto the console till now and after 10 seconds what will happen immediately after 10 seconds we got "This is logged after the promise has been resolved" over here and we also get promise resolve value over here.
- So basically our JavaScript program, JS engine was waiting for promise to resolve when you use this async await
- Once the promise is resolved whether it takes 10 seconds 20 seconds 40 seconds it will wait, the program will wait over here and it will only go to the next line once the promise is resolved, once basically you have got this value this await has got this value inside this

## What happens in the case below?

```
async function getData() {
  console.log("Will this be printed immediately or wait untill the promise gets resolved?");
  const data = await p;
  console.log(data);
  console.log("This is logged after the promise has been resolved");
}
```
- Yes, "Will this be printed immediately or wait until the promise gets resolved?" will quickly be printed and then once this is printed then at next line the program waits for 10 seconds and then "This is logged after the promise has been resolved" is printed.

## What happens in the case below?
```
async function getData() {

  console.log("Will this be printed immediately or wait untill the promise gets resolved?");

  const data = await p;

  console.log(data);
  console.log("This is logged after the promise has been resolved");

  const data2 = await p;

  console.log(data2);
  console.log("This is logged after the promise has been resolved");
}
```

- What will happen now? Will our program wait two times or will it run parallelly, how will it happen how will this whole thing be printed?
- After 10 seconds after 10 seconds both the promises are resolved and everything after "Will this be printed immediately or wait untill the promise gets resolved?" is printed together.
- What happenes is we had the same promise and we were resolving it twice and you would have thought that okay the program will wait for 10 seconds over here then it will wait for 10 seconds over here so after 10 seconds these logs will be printed and after 10 after, 20 seconds these logs will be printed. It will not work like that.

## Lets make it even more complicated. What happens in the case below?
```
const p = new Promise((resolve, reject) => {
  setTimeout(() => resolve("1"), 10000);
});

const p2 = new Promise((resolve, reject) => {
  setTimeout(() => resolve("2"), 5000);
});

async function getData() {
  console.log("First");
  const data = await p;
  console.log("Promise 1 resolved");
  console.log(data);

  const data2 = await p2;
  console.log("Promise 2 resolved");
  console.log(data);
}
```
- P1 is 10 seconds and P2 is five seconds.
- "First" will be printed immediately quickly but what will happen to p1 promise,  JavaScript should wait over here for 10 seconds and over here for p2 promise which is 5 seconds i.e after 15 seconds, everything after promise 2 should be printed. So what will happen JavaScript will get confused.
- After 10 seconds all of these it's printed isn't it interesting right. P1 promise was resolved in five seconds but still the lines below had to wait for 10 seconds to resolve.

## Lets reverse the case!
```
const p = new Promise((resolve, reject) => {
  setTimeout(() => resolve("1"), 5000);
});

const p2 = new Promise((resolve, reject) => {
  setTimeout(() => resolve("2"), 10000);
});

async function getData() {
  console.log("First");
  const data = await p;
  console.log("Promise 1 resolved");
  console.log(data);

  const data2 = await p2;
  console.log("Promise 2 resolved");
  console.log(data);
}
```
- Lets make P1 promises five seconds and make the P2 promises 10 seconds
- So "First" is printed. Now after five seconds our first promise is rolled and it is printed onto the console after total 10 seconds the other promise is rolled and printed. 
