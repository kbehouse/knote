## Why use arrow function:
 [Arrow function MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
 
 Correct: You cound use *this*.setState()
 ```
 path_pending.then((path_result) => {
        this.setState({ path: path_result });
 })
 ```
 
 Error: You could **NOT** use *this*.setState()
  ```
 path_pending.then(function (path_result){
        this.setState({ path: path_result });
 })
 ```
 
 
