#### React Returns
- A component can return only a single top level xml, because it makes easy to do reconciliation. (Reconciliation is the process of figuring out what DOM updates need to happen as the application grows)
#### Re-rendering in React
- install react dev tools extension in the chrome and enable this option
![[Pasted image 20251207135513.png]]
![[Pasted image 20251207135709.png]]
- One way to minimize re-render is to pushing state variables down to the components. While using this method, we must keep the state variables as down to DOM tree as possible (not at lowest, cause if two leaf components require same state variable then its better to keep that state variable in the parent of that two component and pass it via pops, cause if we store the state variable in any one of leaf component we can't pass it like from one leaf component to its parent and from that parent to other leaf component. The solution to this is available and will be learned in State Management).
![[Pasted image 20251207192522.png]]
- Another way is to use memo hook (this is React.memo don't get confused it with useMemo, both are different), it basically lets you skip re-rendering a component when its props are unchanged. Read documentation for React.memo here: https://react.dev/reference/react/memo
![[Pasted image 20251207192556.png]]
#### Keys in React
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
#### Wrapper Component
![[Pasted image 20251207153923.png]]
- Option 1:
![[Pasted image 20251207190523.png]]
- Option 2 (Recommended): (make sure `children` spelling is correct)
![[Pasted image 20251207191945.png]]
example 2:
![[Pasted image 20251207192132.png]]
#### Some Hooks
![[Pasted image 20251207212603.png]]
**Lifecycle Features:** Each component in React has a lifecycle which you can monitor and manipulate during its three main phases: Mounting, Updating, and Unmounting, with an additional phase for Error Handling. In old react, components were written in the classes which was extending (inheriting) react's pre-written classes (same as Flutter) and there were special methods in the classes to monitor and manipulate the the components during these phases, e.g. **`componentDidMount()`**, **`onComponentMount()`** **`shouldComponentUpdate()`**, **`componentDidUpdate()`**, **`shouldComponentUpdate()`**, **`componentWillUnmount()`**, **`render()`**, etc.
1. **useEffect:** This hook runs a function when the component mounts. This can be used to run particular functionalities which needs to run only once when component mounts. There is also a Dependency array which tell when to run function, empty array means to run on first mount, if we put any state variable there, for example if we put *todos* then the function will run when *todos* change, likewise we can put list of state variables.
![[Pasted image 20251207220516.png]]
2. 