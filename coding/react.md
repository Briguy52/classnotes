# Thinking in React

Wow, the entire [website](https://facebook.github.io/react/docs/thinking-in-react.html) is actually hosted with GitHub pages- that's super cool! 

## Step 1: Break your UI mockup into components

### Single Responsibility Principle

Each **component** should only be responsible for one thing

See this graphic from the website:

![](https://facebook.github.io/react/img/blog/thinking-in-react-components.png)

Which corresponds to a little component hierarchy like this:

  * `FilterableProductTable`
    * `SearchBar`
    * `ProductTable`
      * `ProductCategoryRow`
      * `ProductRow`

## Step 2: Build a static version in React

Start by defining all your components

```
var myComponent = React.createClass({
 render function() {
  const TITLE_TEXT = 'Welcome!'
  var name = <span style={{'color': 'red'}}> {this.props.product.name} </span>;
  return (
    <h1> {TITLE_TEST} </h1>
    <div> {name} </div>
   );  
  }  
)};

var PRODUCT = {name: 'Womp', info: 'hello hello'}

ReactDOM.render(
 <myComponent product={PRODUCT}>
 document.getElementById('container') 
 );
```

### Props and State

Don't use state unless there's interaction. You can and should pass props from parents to children

Highest level component (for example, the entire table component) can receive a prop corresponding to all the data. It then gets to pass parts of that down to the other components. 

Note: React has a **one way data flow (binding)**

#### Building- bottom or top?

For simple projects, you can do top down

For larger projects, it's better to start from the bottom and write tests before combining things

#### Updating

Calling `ReactDOM.render()` re-renders the static site 

