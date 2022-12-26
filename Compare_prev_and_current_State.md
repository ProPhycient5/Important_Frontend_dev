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
