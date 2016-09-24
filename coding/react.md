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
