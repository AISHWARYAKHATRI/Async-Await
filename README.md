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
