Following code is how can we access previous and current value of state variable in ReactJS using `useRef` and `useState` hook.

- The following example is from [ReactJS official documentation](https://reactjs.org/docs/hooks-faq.html#how-to-get-the-previous-props-or-state)
```
function Counter() {
  const [count, setCount] = useState(0);
//the useRef Hook allows you to persist data between renders
  const prevCountRef = useRef();
  useEffect(() => {
    //assign the ref's current value to the count Hook
    prevCountRef.current = count;
  }, [count]); //run this code when the value of count changes
  return (
    <h1>
      Now: {count}, before: {prevCountRef.current}
      {/*Increment  */}
      <button onClick={() => setCount((count) => count + 1)}>Increment</button>
    </h1>
  );
}
```

- There is another way of doing so by using `usePrevious` custom hook.

```
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value; //assign the value of ref to the argument
  },[value]); //this code will run when the value of 'value' changes
  return ref.current; //in the end, return the current ref value.
}
export default usePrevious;

function Counter() {
  const [count, setCount] = useState(0);
  // 👇 look here
  const prevCount = usePrevious(count)

  return <h1> Now: {count}, before: {prevCount} </h1>;
}
```
[Ref for above code](https://blog.logrocket.com/accessing-previous-props-state-react-hooks/)
- Here, the vital role is `useRef` hook, which we will go more deeper how `useRef` works.
- Whenever, we learn about `useRef`, we often related to the only way to access DOM in `ReactJS`. This is the best way but not the only way service that `useRef` provides.
- It provides an analogous way of implementing varibale instantiation within functional component.
- Moreover, data remains immutable across various UI renders, thus, help retreiving the initial data. 

```
// class component 
class Count extends React.Component {

   constructor() {
     this.specialVariable = "SPECIAL_VARIABLE"
   }

  render() {
    return null
  }
}
```
- Count has its own copy of instance variables `specialVariable` instance variable.
- The above same thing can be done in functional component using `useRef`.
```
// functional component 

function Count() {
  const specialVariable = useRef("SPECIAL_VARAIBLE"); //store initial value into the specialVariable Hook. 
  return null
}
```
- The beauty💖 of `useRef` is that, it creates own propety i.e. `current` and store initial value within it. 
 
`useRef("INITIAL_VALUE")`, and it returns an object with a current property `{current: "INITIAL_VALUE"}`.

- So, whenever value was initially passed into the `useRef` Hook, that is saved to the current property of the ref object.
```
function Count() {
  const specialVariable = useRef("SPECIAL_VARAIBLE");
  // specialVariable resolves to {current: "SPECIAL_VARIABLE"}
  console.log(specialVariable); //for debugging purposes, log out its value
  return null;
}
//console log - {current: "SPECIAL_VARAIBLE"}
```
- The interesting fact about this property of `useRef` is unlike a normal variable, the specialVariable ref object is not recomputed when the Count component is re-rendered. With the useRef Hook, the value saved in the ref object is kept the same across re-renders. The value is neither lost nor recomputed; it remains the same.
- So, as normal JS object, the only way to update the value of property is by assigning the value explicitly. 
```
 i.e., specialVariable.current = "NEW_SPECIAL_VARIABLE.
```

So the above explanation throws some light about how `useRef` works. But next we will go step by step how the above code `usePrevious` custom hooks work.

