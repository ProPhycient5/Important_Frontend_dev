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

- So the above explanation throws some light about how `useRef` works. 
- But next we will go step by step how the above code `usePrevious` custom hooks work.😏

- Let us look at the below code again holistically.

```
// custom hook for getting previous value 
function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  },[value]);
  return ref.current;
}

// the App where the hook is used 
function Counter() {
  const [count, setCount] = useState(0);
  // 👇 look here
  const prevCount = usePrevious(count)

  return <h1>Now: {count}, before: {prevCount}</h1>;
}
```

1. So when the `Counter`app is rendered for the first time.
 - `useState` Hook is invoked.
 - State variable `count` and its corresponding update function `setCount` are initiated.
 - So, `count` = 0
![useRef_1](https://user-images.githubusercontent.com/71059909/210546980-d32cf4e9-df5b-46eb-bc77-2ab613171236.JPG)

2. Going by the line-by-line execution as default trait of `JS`.
 - Custom hook `usePrevious` is invoked with the current value of `count` as 0.
![useRef_2](https://user-images.githubusercontent.com/71059909/210548245-32391b3b-dc1d-4064-9c20-78d587694b25.JPG)

3. As we know when any function is invoked, a brand new EC(excecution context) is created inside global EC.
 -  React creates a new `useRef` instance.
 -  The initial value of this Hook is `undefined` because the `current` key doesn’t exist at first place.
 ![useRef_3](https://user-images.githubusercontent.com/71059909/210549425-47916aee-f342-4bfd-b4bf-6861d2a18fe2.JPG)
 
4. This next step is quite crucial where most people overlook.
 - React doesn’t execute the useEffect call, instead, the `return` is executed.
 - `useEffect` Hook is invoked only after the component from which it is called has been rendered. 🙄
 - Which also means the code(here it is `HTML`) inside the return scope of the component must be executed first.
 ![useRef_4](https://user-images.githubusercontent.com/71059909/210553899-5d6f5a8e-b7f7-44f9-95e4-83ba51ec8228.JPG)

5. Now in this step, the custom hook EC is executed and its corresponding EC is deleted and the control is back to the `return` statement. 
   - `prevCount` variable holds the value as `undefined`.
   ![useRef_5](https://user-images.githubusercontent.com/71059909/210555736-5a102ca4-3f9f-417a-aeee-6175f11e4d7e.JPG)
   
6. Here, the return value of the component is evaluated.
   - `<h1>Now: {count}, before: {prevCount}</h1>` is converted to `<h1>Now: 0, before: undefined</h1>`, where `count` & `prevCount` are 0 and `undefined`, respectively.  
    ![useRef_6](https://user-images.githubusercontent.com/71059909/210556852-a6b3f413-f047-4842-a67f-c836eeef3d26.JPG)
    
7. `useEffect` inside `usePrevious` Hook is now invoked asynchronously after functional component is rendered, allowing browser to paint DOM changes.
    ![useRef_7](https://user-images.githubusercontent.com/71059909/210606600-d41257b6-71d8-4508-aaf9-cdb26c1ae9b9.JPG)
    - Below is the effect function.
    ```
    useEffect(() => {
     ref.current = value;
    },[value]);
    
    ```
    - The line within the useEffect function updates the `current` property of the `ref` object to `value`.
    - Custom hook is initially invoked, the value is 0. In this current flow, remember usePrevious has only been called once with the initial value of 0.
    - In this case, the value is `0`. In this current flow, remember `usePrevious` has only been called once with the initial value of `0`.

![useRef_8](https://user-images.githubusercontent.com/71059909/210615191-02788c5b-7c4d-4293-af7c-7fad89208fd4.JPG)

8. 

 


