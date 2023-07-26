---
layout: post
title:  "Today I Learned: Use refs to hold values that does not require a re-render"
date:   2023-07-26 21:09:00 +0530
---
> When you want a component to “remember” some information, but you don’t want that information to trigger new renders, you can use a ref.
>
> *[react.dev](https://react.dev/learn/escape-hatches)*

Imagine a basic counter component that consists of a *count* value and buttons to increase or decrease the count. To achieve this, we'll store the `counter` value in state and use its setter function to increment or decrement the value.
```
function Counter() {
  const [counter, setCounter] = useState(0);
  function increment() {
    setCounter(counter + 1);
  }
  function decrement() {
    setCounter(counter - 1);
  }
  return (
    <>
      <h1>Counter: {counter}</h1>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </>
  );
}
```

When the counter value changes, the component re-renders, and the updated counter value is shown. Now, what if we substitute the state variable with a `ref`?
```
function RefCounter() {
  const counter = useRef(0);
  function increment() {
    counter.current += 1;
    console.log("counter: ", counter.current);
  }
  function decrement() {
    counter.current -= 1;
    console.log("counter: ", counter.current);
  }
  return (
    <>
      <h1>Counter: {counter.current}</h1>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </>
  );
}
```

The counter value gets updated with each increment or decrement, but the updated value is not immediately visible on the page. This occurs because React does not trigger a re-render of the component when refs are updated. The updated value will become visible on the page only after the next re-render.
```
function DualCounter () {
  const refCounter = useRef(0)
  const [stateCounter, setStateCounter] = useState(0)

  function incrementRefCounter () {
    refCounter.current += 1
  }

  function decrementRefCounter () {
    refCounter.current -= 1
  }

  function incrementStateCounter () {
    setStateCounter(stateCounter + 1)
  }

  function decrementStateCounter () {
    setStateCounter(setStateCounter - 1)
  }

  return (
    <>
    <h1>Ref counter: {refCounter.current}</h1>
    <button onClick={incrementRefCounter}>+</button>
    <button onClick={decrementRefCounter}>-</button>
    <h1>State counter: {stateCounter}</h1>
    <button onClick={incrementStateCounter}>+</button>
    <button onClick={decrementStateCounter}>-</button>
    </>
  )
}
```
In the given scenario, the updated value of `refCounter` becomes visible only when `stateCounter` is updated. This is because updating the `stateCounter` triggers a re-render of the component, reflecting the changes in the UI.

Compared to `state`, `refs` have distinct characteristics or limitations: they are mutable, they don't trigger re-renders, and they are inaccessible during the rendering process. They are beneficial for storing values that do not require re-renders. For instance, the [React docs](https://react.dev/learn/referencing-values-with-refs#example-building-a-stopwatch) provide an example of using `refs` to store the interval id for a stopwatch component.
```
function Stopwatch() {
  const [startTime, setStartTime] = useState(null);
  const [now, setNow] = useState(null);
  const intervalRef = useRef(null);

  function handleStart() {
    setStartTime(Date.now());
    setNow(Date.now());

    clearInterval(intervalRef.current);
    intervalRef.current = setInterval(() => {
      setNow(Date.now());
    }, 10);
  }

  function handleStop() {
    clearInterval(intervalRef.current);
  }

  let secondsPassed = 0;
  if (startTime != null && now != null) {
    secondsPassed = (now - startTime) / 1000;
  }

  return (
    <>
      <h1>Time passed: {secondsPassed.toFixed(3)}</h1>
      <button onClick={handleStart}>
        Start
      </button>
      <button onClick={handleStop}>
        Stop
      </button>
    </>
  );
}
```
