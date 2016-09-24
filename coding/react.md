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

### Brief example- `Application.js` 

See change [here](https://github.com/hack-duke/hackduke-portal/commit/a65d4414d55175bd8d51f202fdf67c81da89e5ae)

#### Before

```
componentWillReceiveProps() {
  // bind to typeform submission
  // make call to server (every time props change)
}
```

#### After

```
constructor() {
  // bind to typeform submission
  // make call to server (once)
}
```

Read about adding state [here](https://facebook.github.io/react/docs/interactivity-and-dynamic-uis.html)

### General ideas about state

Create some state in your constructor:

```
this.state = {
 liked: false
}
``` 

Modify (set) state with `setState(data, callback)`:

`this.setState({liked: !this.state.liked});`

#### Keep as many components stateless as possible!

One pattern- have stateless components display data and a state component above that passes in state as props

#### What goes in state?

Small things that would trigger a UI update

#### What shouldn't go in state? 

1. Computed data (do this in render()) 
2. Components
3. Duplicate info from props

## Step 3: Identifying minimal representation of UI state

### Ask 3 questions

#### 1. Is it passed from parent as a prop? 

> Not state

#### 2. Does it remain unchanged?

> Not state

#### 3. Can you compute it from other props or data?

> Not state

(kinda like functional dependencies hahaha)

## Step 4: Where should your state live?

For a given piece of state (ex. search text) figure out which components use it

If there's multiple, is there a shared parent that can house the state and pass it down as a prop?

Pro tip: add a `getInitialState()` method that returns what the state is at the start!


