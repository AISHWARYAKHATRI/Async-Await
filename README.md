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

## The Catch! Is the program actually waiting here or not when we use await, what is happening behind the scenes?
- People don't understand how promise execution behavior is working is the program.
- JS engine was waiting for promise to be resolved over here.
- "JS engine was waiting for promise to be resolved". What does it actually mean?
- So JS engine just like time tied does not wait for anything it is 200% true.
- JS engine just appears to be waiting over here.
- JS engine is not waiting over here if it has not it is not consuming memory over here, it has not actually occupied the call stack
- If waiting was the case then our program or our page will freeze our page. But it has not frozen our page.
- So this statement that JS engine was waiting is not true at all right.
- It looks like the JS engine is waiting but it is not true at all.

## Now if the program does not wait then what does it do actually how does it work behind the scenes?
- When this function will be executed it will go line by line because JavaScript is a synchronous single threaded language.
- Call stack is empty right. Basically call stack is the place where every function works right it will come and it will work it will execute our code.
- As soon as this getData() function is called, so it it sees that okay there are two promises which needs to be resolved there and these promises are taking their own time in resolution.
- So basically there are two promise right P1 and P2, they will be resolved at some point of time after 5 seconds after 10 seconds or after we don't know what time it will resolve.
- Initially the call stack is empty over here as soon as we call this getData function, so this getData() function will come inside your call stack right now, because JavaScript is is a synchronous single, it has only one thread, it has only one call stack so it will start executing all of this one by one.
- Its will start executing this lines one by one so first of all it will execute the line and it will log "First" to the console.
- It will see that there is a await P1 over here now there are two cases, will this getDta() stay over here in the call stack and wait for the promise to resolve?
- The answer is no.
- JavaScript does not wait for anything and this handle promise function is execution will suspend and this will move out of call stack it will not block the main thread, it will not freeze your page so that if any other events are happening they can execute inside call stack because JavaScript just have one small stack.
- So when it sees a await function, the function execution suspends. It will wait till this P1 is resolved and once this P1 is resolved then only it will move ahead .
- Here P1 will be resolved after 5 Seconds. This call stack will remain empty but after five seconds, the getData() function will again come in call stack and it will again start executing but this time it will start executing from where it left.
- And then it will see whether this P2 is resolved or not it sees that P2 is not yet resolved because it has just been five seconds right and the P2 will resolve after 10 seconds and the P2 will result after 10 seconds. 
- What it will do it will again suspend and will not block the call stack. 
- Basically it will again suspend the execution.
- P2 is resolved after 10 seconds now getData() will again come back to call stack, getData() will again come back to call stack and it start executing from the place it left.
- "First" will be logged immediately and then after five seconds "Promise 1 resolved" and this Val will be resolved after five seconds and then the program will suspend, the execution will suspend and then after 10 seconds the getData() will again start the execution.

