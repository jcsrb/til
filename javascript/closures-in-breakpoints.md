# Closures in Breakpoints

testing out code in the breakpoint REPL is nice, but make sure you have all the data you need. Make sure you actually use the variable you want to have access to.

### Nope
```js
var doSomething = function(id) {
  ApiService.callAPI().then(function(data){
    debugger // id doesn't exist here if its not used  
  });
}
```

### Yep
```js
var doSomething = function(id) {
  ApiService.callAPI().then(function(data){
    debugger // now it exists 
    console.log(id);
  });
}
````
### Yep
```js
var doSomething = function(id) {
  ApiService.callAPI().then(function(data){
    debugger // now it exists 
    return id;
  });
}
```

More on this http://stackoverflow.com/questions/111102/how-do-javascript-closures-work
