### React Returns
- A component can return only a single top level xml, because it makes easy to do reconciliation. (Reconciliation is the process of figuring out what DOM updates need to happen as the application grows)
### Re-rendering in React
- install react dev tools extension in the chrome and enable this option
![[Pasted image 20251207135513.png]]
![[Pasted image 20251207135709.png]]
- One way to minimize re-render is to pushing state variables down to the components. While using this method, we must keep the state variables as down to DOM tree as possible (not at lowest, cause if two leaf components require same state variable then its better to keep that state variable in the parent of that two component and pass it via pops, cause if we store the state variable in any one of leaf component we can't pass it like from one leaf component to its parent and from that parent to other leaf component. The solution to this is available and will be learned in State Management).
![[Pasted image 20251207192522.png]]
- Another way is to use memo hook (this is React.memo don't get confused it with useMemo, both are different), it basically lets you skip re-rendering a component when its props are unchanged. Read documentation for React.memo here: https://react.dev/reference/react/memo
![[Pasted image 20251207192556.png]]
### Keys in React
- Follow the example:
![[Pasted image 20251207145510.png]]
![[Pasted image 20251207152010.png]]
- Now when we run this code, we get this error, visit https://react.dev/learn/rendering-lists#keeping-list-items-in-order-with-key to read documentation
![[Pasted image 20251207152216.png]]
This error basically says, You need to give each array item a `key` — a string or a number that uniquely identifies it among other items in that array:
`<Todo key={person.id} ... />`
Keys tell React which array item each component corresponds to, so that it can match them up later. This becomes important if your array items can move (e.g. due to sorting), get inserted, or get deleted. A well-chosen key helps React infer what exactly has happened, and make the correct updates to the DOM tree. Rather than generating keys on the fly, you should include them in your data like this:
![[Pasted image 20251207152826.png]]
![[Pasted image 20251207153717.png]]
### Wrapper Component
![[Pasted image 20251207153923.png]]
- Option 1:
![[Pasted image 20251207190523.png]]
- Option 2 (Recommended): (make sure `children` spelling is correct)
![[Pasted image 20251207191945.png]]
example 2:
![[Pasted image 20251207192132.png]]
### Side Effects
![[Pasted image 20251214103657.png]]
- These side effects needs to be put outside the react component re-rendering
### Some Hooks
![[Pasted image 20251207212603.png]]
**Lifecycle Features:** Each component in React has a lifecycle which you can monitor and manipulate during its three main phases: Mounting, Updating, and Unmounting, with an additional phase for Error Handling. In old react, components were written in the classes which was extending (inheriting) react's pre-written classes (same as Flutter) and there were special methods in the classes to monitor and manipulate the the components during these phases, e.g. **`componentDidMount()`**, **`onComponentMount()`** **`shouldComponentUpdate()`**, **`componentDidUpdate()`**, **`shouldComponentUpdate()`**, **`componentWillUnmount()`**, **`render()`**, etc.
##### useState:
This let's us describe the state of our app. Whenever state updates, it triggers a re-render, which finally result in a DOM update.
##### useEffect:
This hook runs a function when the component mounts. This can be used to run particular functionalities which needs to run only once when component mounts. There is also a Dependency array which tell when to run function, empty array means to run on first mount, if we put any state variable there, for example if we put *todos* then the function will run when *todos* change, likewise we can put list of state variables.
![[Pasted image 20251207220516.png]]
![[Pasted image 20251208223403.png]]
- we can't use async as top function in useEffect like this:`useEffect(async () => {...}, []);` but inside to top level function we can use async function.
- one way what we can do is define async function outside and just call it in the useEffect like this:
![[Pasted image 20251214114559.png]]
- but in this approach there is a issue, think as the useEffect runs and request goes to server but suppose there is an issue ongoing with server and it took some time to respond and it that time useEffect got triggered again due some UI activity (suppose you put any state variable in dependancy array of useEffect that variable changes and triggers useEffect again), so now second request goes to server, now think if server sends a response for second request first and after some time once its issue resolved send the response for that first request.
- That's why the better approach would be if your top function is async then either use `.then()` function for first async activity, and inside `then()` use async function, or the approach would be using the hook which could handle top async function e.g. `useAsyncEffect`(npm package). To know more about this issue in detail read this blog: https://devtrium.com/posts/async-functions-useeffect
##### useMemo: 
`useMemo` is a React hook that **memoizes (caches) the result of an expensive calculation**, returning the stored value on re-renders unless its dependencies change. e.g.
![[Pasted image 20251222215355.png]]
![[Pasted image 20251222215459.png]]
##### useCallback:
`useCallback` is a React Hook that memoizes (caches) a function, returning the _same_ function instance across re-renders unless its dependencies change, which prevents unnecessary re-renders in child components.
![[Pasted image 20251222215816.png]]
- reference equality: 
![[Pasted image 20251222220550.png]]
![[Pasted image 20251222220431.png]]
basically in simple terms, using useMemo we memorize the result of a heavy calculation (treate it as value inside a variable), but what if we have memorize a function, (in js we can store a function inside a variable, but like numbers or strings it is not able to differentiate their value, like if a=5 and b=5 then `a===b` will be true because it is comparing it by its value, if we store functions like `a= (a,b) => {return a+b}` and `b= (a,b) => {return a+b}` then `a===b` will be false even if function defination is same, because it is now comparing them with their reference, so if we use useMemo for function, it will still run that function on each render) then we use useCallback
##### useRef:
Suppose you have to change/override a particular thing which react rendered on the screen. The one way of doing it is by using `document` object (`document.getElementById('').innerHTML..`). This will work but is not a best practise, because you will change the value (or state), it will even reflect on the screen too, but the react will still think it as old value beacause thats what he put there, and its kind of confusing thing to the react.
![[Pasted image 20260109221858.png]]
So useRef basically a hook which gives us the reference of the DOM element.
![[Pasted image 20260109222100.png]]
