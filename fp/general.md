## Maybe Type

In order to deal with uncertainy, we make certain functions return a Maybe

A Maybe value is really either:
Just(result)
Nothing()

So it can either be Just(result) which means everythign went well, or
it will be Nothing(), meaning not everything went well.

The reason why this is better than just throwing an error is:
  the error stops everthing and the program is done... now we have to catch that
  returning null or undefined is harded to handle downstream in our composition pipeline,

Its better to return a Nothing() value, which is a safer way of saying null or undefined.
We can still continueing composing and calling functions on a Nothing() because it is a monad that
has map methods and so on.

### getOrElse
Another benefit to the Maybe type is it often has a getOrElse method on it. Example:
```js
const name = username.getOrElse('Enter first name');
```
If the username has a Just value, it will be the a value, if its a Nothing value, than you can 
recover with the 'OrElse'.

### Defensive Null Checking vs Maybe
It is common to sprinkle null checks all over your code like this:
```js
function getCountry(student) {    
  let school = student.school();    
  if(school !== null) {       
    let addr = school.address();       
    if(addr !== null) {          
      return addr.country();       
    }    
  }    
  return 'Country does not exist!'; 
}
```

With the Maybe type, you can avoid having to catch every spot where it might go wrong and handle it at the end, like this:
```js
const getCountry = student => student
  .map(R.prop('school'))
  .map(R.prop('address'))
  .map(R.prop('country'))
    .getOrElse('Country does not exist.')
```
