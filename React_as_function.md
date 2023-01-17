- A React component is simply a function(or boiled down to mathematical function).
- We all know f(x) = y notation.
  where f is a function, x is the input (or parameters) and y as the the output.
- Similarly, a React application is the f in that equation, x is your React state and y is the resulting user interface.

- Here, how it looks the body of a such function f:
![react_1](https://user-images.githubusercontent.com/71059909/212899631-dfd208d2-9b94-46f3-82be-d99724a15558.png)

-- When is the function called?
The React function is called whenever x changes. Whenever the React state changes, the function is "called" and a new UI is returned. Now, technically, if a 
piece of state is only scoped to a child component (like let's say f1 in the code above), React is aware enough to call just that sub-function and not the whole thing.

Credit:
* [Zeke](https://github.com/zekehernandez)
* [this article](https://www.zekehernandez.com/posts/react-applications-are-functions)
