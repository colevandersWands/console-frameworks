# Console Frameworks

words

### Readings
* [learning objectives](#learning-objectives)
* [test cases](#test-cases)
* [testing framework](#testing-framework)

### Challenges
* variables
    * [value swap](#value-swap) (completed example)
    * [block scope 'let'](#block-scope-let)
    * [block scope 'var'](#block-scope-var)
* reference types
    * [no-copy array](#no-copy-array)
    * [yes-copy array](#yes-copy-array)
    * [dots vs brackets](#dots-vs-brackes)
* conditionals
    * [or and truthyness](#or-and-truthiness)
    * [if, else if, else](#if-else-if-else)
    * [nested conditionals](#nested-conditionals)
* iteration
    * [accumulating](accumulating)
    * [repeating until](#repeating-until)
    * [once per thing](#once-per-thing)
    * [breaking down](#breaking-down)

---

## Learning Objectives

* working within existing code
* first introduction to frameworking
* generalized testing script
* understanding code behavior with test cases
* writing passing test cases
* {name, args, expected} test format

[TOP](#console-frameworks)

---

## Test Cases

A snippet's behavior is what has changed in your program _after_ the snippet has run, implementation is the lines of code that make up the snippet.  In the last exercises on tracing, logging & asserting you saw this without the vocab.  The 'expected' variable described the snippet's _behavior_, while the logs and asserts helped to understand the snippet's _implementation_. This exercise will take a deeper look at implementation, breaking down dense snippets of code into longer pieces of code that illustrate the same implementation one operation at a time.

To make sure that you don't make any mistakes while refactoring the original snippet, we have provided test cases for each one.  With every small change you make, you will run the tests again.  If they pass you've done a correct change.  If any tests fail, revisit your last change and see what went wrong!

Practically speaking you can think of test cases as just inputs and outputs.   What values did variables have at the top of a snippet, and what values do they have after the snippet?  To use vocabulary, what are the __arguments__ and what is the __expected__ output:

```js
const test_case = {name:'helpful name', args:['different', 'inputs'], expected:'the expected output'};
```

[TOP](#console-frameworks)

---

## Testing Framework

So you can focus on writing test cases, we've provided the framework below to log a helpful message for every one of your failing test cases.  This message will include the name of the failing case, the expected value and the actual value.  In this exercise if a test fails it's because the test is wrong, not because the code is broken.  Remember, you're describing what the code _does_ do, not what it _should_ do.

To complete the exercises paste this framework into the devtools console.  Refactor the snippet you're studying so it works with the framework, start a new empty test case with your best guess.  Hit enter to see if you're right!  

It is possible to build the test cases just with trial and error but you won't be doing yourself any favors.  Do you best effort to predict the correct expected values before running the tests.



simple_framework: {
  const test_case = ;
  const log = [];

  let actual; { 
      // paste snippet challenge here
  };

  // framework magic happens down here
  const expected = _case.expected;
  let pass = null;
  if (typeof expected === 'object') {
    const _actual = JSON.stringify(actual);
    const _expected = JSON.stringify(expected);
    pass = _actual === _expected;
  } else {
    pass = actual === expected;
  };
  if (pass) {
    console.groupCollapsed(`%c ${_case.name}: \n`, 'color:green', log);
  } else {
    console.groupCollapsed(`%c ${_case.name}: \n`, 'color:red', log);
    console.log(`%c   actual: ${typeof actual},`, 'color:red', actual);
    console.log(`%c   expected: ${typeof expected},`, 'color:blue', expected);
    console.groupEnd();
  };
};


[TOP](#console-frameworks)


---
---

## Variables 

### Value Swap

[on pytut](https://goo.gl/27fPqs)  
[interactive example](https://github.com/elewa-academy/value-swap)  

original snippet:
```js
{
  const expected = ;                const log = [{expected}];

  let a = ;                        
  let b = ;                        
  let temp = ;                      log.push({a,b,temp});
   
  temp = b;                         log.push({temp});
  b = a;                            log.push({b});
  a = temp;                         log.push({a});

  const actual = temp;              log.push({actual});
  
  console.assert(actual === expected, log);
}
```
the values:
```js
a:1, b:2, temp:3            --> ?
a:'a', b:'b', temp:'temp'   --> ?
a:true, b:false, temp:null  --> ?
a:'', b:0, temp:undefined   --> ?
```
your notes:

---

### Block Scope 'let'

[on pytut](https://goo.gl/Ym63eU)  
[interactive example](https://github.com/elewa-academy/nesting-blocks-let)  

the code:
```js
{
  const expected = ;              const log = [{expected}];

  let a = ;                        
  let b = ;  
  let c = ;                       log.push({a,b,c});

  {
    let a = b;                    log.push({a});
    {
       a = c;                     log.push({a});
    }
    a = a;                        log.push({a});
  }

  const actual = a;               log.push({actual});
  
  console.assert(actual === expected, log);
}
```
the values:
```js
a:1, b:2, c:3         --> ?
a:'', b:0, c:false    --> ?
a:true, b:false, c:9  --> ?
```
your notes:  

---

### Block Scope 'var'  

[on pytut](https://goo.gl/rJaPQo)  
[hoisting](https://github.com/elewa-academy/hoisting) 
[hoisting & blocks](https://github.com/elewa-academy/hoisting-and-blocks)
[let vs var](https://github.com/elewa-academy/block-scope-let-vs-var)  


the code:
(refresh the page each time before running this exercise)  
```js
{
  const expected = ;              const log = [{expected}];
                       
  let b = ;  
  let c = ;                       log.push({a,b,c});

  {
    var a = b;                    log.push({a});
    {
       a = c;                     log.push({a});
    }
    a = a;                        log.push({a});
  }

  const actual = a;               log.push({actual});
  
  console.assert(actual === expected, log);
}
```
the values:
```js
b:2, c:3         --> ?
b:0, c:false     --> ?
b:false, c:9     --> ?
```
your notes:  

---
---

## Reference Types


### no-copy arrays  

[on pytut](https://goo.gl/xX64Cg)  
[more about this](https://github.com/elewa-academy/reference-vs-value)

the code:
```js
{
  const expected = [];            const log = [{expected}];
                       
  const a = [];                     
  const b = [];                      
  const x = ;
  const y = ;                     log.push({a:a.slice(),b:b.slice(),x,y});
  
  a.push(b);                      log.push(a.slice());
  b.push(x);                      log.push(b.slice());
  a.push(b);                      log.push(a.slice());
  b.push(y);                      log.push(b.slice());
  a.push(b);                      log.push(a.slice());

  const actual = a;               log.push({actual:actual.slice()});
  
  const assert_act = JSON.stringify(actual);
  const assert_exp = JSON.stringify(expected);
  console.assert(assert_act === assert_exp, log);
}
```
the values:
```js
x:2, y:3         --> ?
x:'x', y:'y'     --> ?
x:null, y:0      --> ?
```
your notes:  

---


### yes-copy arrays

[on pytut](https://goo.gl/UCT8Co)  
[more about this](https://github.com/elewa-academy/reference-vs-value)

the code:
```js
{
  const expected = [];            const log = [{expected}];
                       
  const a = [];                     
  const b = [];                      
  const x = ;
  const y = ;                     log.push({a:a.slice(),b:b.slice(),x,y});
  
  a.push(b.slice());              log.push(a.slice());
  b.push(x);                      log.push(b.slice());
  a.push(b.slice());              log.push(a.slice());
  b.push(y);                      log.push(b.slice());
  a.push(b.slice());              log.push(a.slice());

  const actual = a;               log.push(actual.slice());
  
  const assert_act = JSON.stringify(actual);
  const assert_exp = JSON.stringify(expected);
  console.assert(assert_act === assert_exp, log);
}
```
the values:
```js
x:2, y:3         --> ?
x:'x', y:'y'     --> ?
x:null, y:0      --> ?
```
your notes:  

---

### Dots vs Brackets

[on pytut](https://goo.gl/2G6nuu)  
[extra resource](https://github.com/elewa-academy/variables-and-types/tree/master/dots-vs-brackets) 

the code:
```js
{
  const expected = [];            const log = [{expected}];

  const arr = [];
  const obj = {a: 1, b: 2};
  const a = ;
  const b = ;                     log.push({arr:arr.slice(), obj, a, b})

  arr.push(obj.a);                log.push(arr.slice());
  arr.push(obj.b);                log.push(arr.slice());

  arr.push(obj[a]);               log.push(arr.slice());
  arr.push(obj[b]);               log.push(arr.slice());

  const actual = arr;             log.push(actual.slice());
  
  const assert_act = JSON.stringify(actual);
  const assert_exp = JSON.stringify(expected);
  console.assert(assert_act === assert_exp, log);
}
```
the values:
```js
a:'a', b:'b'      --> ?
a:'b', b:'a'      --> ?
a:1, b:2          --> ?
a:2, b:1          --> ?
```
your notes:  

---
---


## Conditionals


### or and truthyness

[on pytut](https://goo.gl/TgqxaR)

the code:
```js
{
  const expected = ;             const log = [{expected}];
                       
  const a = ;                     
  const b = ;
  let x = null;
  let y = null;                  log.push({a, b, x, y});
  
  let condition = a || b;        log.push({condition: Boolean(condition) });
  if (condition) {                 
    x = b;                       log.push({x});
    y = a || b;                  log.push({y});
  } else {
    x = a;                       log.push({x});
    y = a || b;                  log.push({y});
  };
  
  const actual = y;
  
  console.assert(actual === expected, log);
}
```
the values:
```js
a:true, b:false      --> ?
a:false, b:true      --> ?
a:0, b:1             --> ?
a:1, b:0             --> ?
a:null, b:false      --> ?
a:false, b:null      --> ?
a:'', b:' '          --> ?
a:' ', b:''          --> ?
a:2, b:3             --> ?
a:3, b:2             --> ?
```
your notes:  

---


### if, else if, else  

[on pytut](https://goo.gl/zhTS2m)

the code:
```js
{
  const expected = ;                const log = [{expected}];
                     
  const a = ;
  const b = ;
  const c = ;
  const result = null;              log.push({a,b,c,result});
  
  const condition_1 = b && a;       log.push({condition_1: !!condition_1});
  const condition_2 = a || c;       log.push({condition_2: !!condition_2});
  if (condition_1) {
    result = condition_1;           log.push({result});
  } else if (condtion_2) {
    result = condition_2;           log.push({result});
  } else {
    result = 'else';                log.push({result});
  };

  const actual = result;
  
  console.assert(actual === expected, log);
}
```
the values:
```js
a:true, b:'', c:1      --> ?
a:false, b:'', c:0      --> ?
a:true, b:'', c:0      --> ?
a:false, b:'', c:1      --> ?
a:false, b:' ', c:1      --> ?
a:true, b:' ', c:1      --> ?
```
your notes:  

---



### nested conditionals

[on pytut](https://goo.gl/D9R3HS)

the code:
```js
{
  const expected = ;                const log = [{expected}];
                     
  const a = ;
  const b = ;
  const result = null;              log.push({a,b,c,result});
  
  if (a) {
    if (b) {
      result = 'w: ' + a + b;       log.push({result});
    } else {
      result = 'x: ' + a + b;       log.push({result});
    };
  } else {
    if (b) {
      result = 'y: ' + a + b;       log.push({result});
    } else {
      result = 'z: ' + a + b;       log.push({result});
    };
  };

  const actual = result;
  
  console.assert(actual === expected, log);
}
```
the values:
```js
a:0, b:0    -->  ?
a:0, b:1    -->  ?
a:1, b:0    -->  ?
a:1, b:1    -->  ?
```
your notes:  

---
---


## Iteration

### accumulating

[on pytut](https://goo.gl/YFn6Zd)

the code:
```js
{
  const expected = ;                const log = [{expected}];
             
  const things = ;
  let result = null;                log.push({things,result});
             
  let i = 0;                        log.push({i});
  while (i < things.length) {       log.push({while_condition: i < things.length});
    result += things[i];            log.push({result});
    ++i;                            log.push({i});
  };

  const actual = result;
  
  console.assert(actual === expected, log);
}
```
the values:
```js
things:[-1, 0, 1]            --> ?
things:[-3, -2, -1]          --> ?
things:[true, false, 1, 0]   --> ?
things:['words', 5, 1e3]     --> ?
things:[]                    --> ?
```
your notes:  

---

### repeating until

[on pytut](https://goo.gl/jbvs2o)

the code:
```js
{
  const expected = ;             const log = [{expected}];
                     
  let a = ;
  let b = ;
  const c = ;
  let steps = 0;                 log.push({a,b,c,steps});
  
  while (a !== b) {              log.push({while_condition: !!(a !== b)});
    a -= c;                      log.push({a});
    b += c;                      log.push({b});
    steps++;                     log.push({steps});
  };

  const actual = steps;
  
  console.assert(actual === expected, log);
}
```
the values:
```js
a:10, b:0, c:1      --> ?
a:8, b:2, c:1       --> ?
a:7, b:5, c:1       --> ?
a:13, b:1, c:3      --> ?
a:0, b:-28, c:7     --> ?
a:15, b:-9, c:3     --> ?
```
your notes:  

---

### once per thing

[on pytut](https://goo.gl/Gbtsu9)

the code:
```js
{
  const expected = ;                   const log = [{expected}];
                     
  const things = ;
  const result = [];                   log.push({things,result:result.slice()});       
  
  let i = 0;                           log.push({i});
  while (i < things.length) {          log.push({while_condition: i < things.length});
    const new_thing = !things[i];      log.push({new_thing});
    result.push(new_thing);            log.push(result.slice());
    i += 1;                            log.push({i});
  };
  
  const actual = result;

  const assert_act = JSON.stringify(actual);
  const assert_exp = JSON.stringify(expected);
  console.assert(assert_act === assert_exp, log);
}
```
the values:
```js
things:[false,true]     --> ?
things:[0,1]            --> ?
things:['',' ']         --> ?
things:[Infinity,NaN]   --> ?
things:[[],{}]          --> ?
things:[[1],{1}]        --> ?
```
your notes:  

---

### breaking down

[on pytut](https://goo.gl/iupw2c)

the code:
```js
{
  const expected = ;                const log = [{expected}];
                     
  const word = ;
  const letters = [];               log.push({word,letters});

  let i = 0;                        log.push({i});
  while (i < word.length) {         log.push({while_condition: i < word.length });
    const letter = word[i];         log.push({letter});
    letters.push(letter);           log.push(letters.slice());
    i = i + 1;                      log.push({i});
  };

  const actual = letters;
 
  const assert_act = JSON.stringify(actual);
  const assert_exp = JSON.stringify(expected);
  console.assert(assert_act === assert_exp, log);
}
```
the values:
```js
word:"word"       --> ?
word:""           --> ?
word:"\n\t"       --> ?
word:"\e\\"       --> ?
word:"3.5"        --> ?
```
your notes:  

---

_the end_

___
___
### <a href="http://janke-learning.org" target="_blank"><img src="https://user-images.githubusercontent.com/18554853/50098409-22575780-021c-11e9-99e1-962787adaded.png" width="40" height="40"></img> Janke Learning</a>
