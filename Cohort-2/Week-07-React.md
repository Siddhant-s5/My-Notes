### Routing
![[Pasted image 20260110151704.png]]
we use react-router-dom for the routing. docs link: https://reactrouter.com/
![[Pasted image 20260110153828.png]]

![[Pasted image 20260110154730.png]]
*location.href gives the full url, we can try to console.log location object*.
![[Pasted image 20260110154658.png]]
The issue in this approach is this will refetch the files from server everytime route changes, but react is a Singlepage application and supports client side routing ( this means it fetches the complete bundle (code of all the pages and everything) in the first request itself ).
That's why we use useNavigate hook provided by reac-router-dom library to navigate
![[Pasted image 20260110160141.png]]
The above code will throw an error as useNavigate needs to use inside a component which inside BrowserRouter. As our buttons are outside the BrowserRouter, this will throw an error.
![[Pasted image 20260110160804.png]]
Now this will work perfectly, as useNavigate is used inside AppBar which is inside BrowserRouter.
There is also one component named Link to navigate, chechout on that too...
*react-router-dom* also gives one more thing i.e. Lazy loading. We know that react fetches a complete bundle on first reload, but think if we have 100 pages then there is a possibility that user won't see/hit all 100 pages, so wouldn't it be nice if we fetch the code of page which user is hitting instead of providing a full bundle all at once, we can incremently give the code of pages i.e. lazy loading.
![[Pasted image 20260110163029.png]]
That Suspense component/API is provided by the React, if we don't use it then react will throw an error, cause when page is getting fetched (asynchronous) it can take any time, so in the meanwhile what should i (react) render on screen, so using Suspense it renders whatever inside the *fallback*.
### Prop Drilling & Context API
![[Pasted image 20260111090821.png]]
*LCA - Lowest Common Anccestor*
The answer is we should push down state as much as possible. Because if the state is changing in say C2, it will trigger the re-render and we don't want C1 to re-render, so if we keep that state in C2 then only C2, C4 & C5 will re-render.
And storing state in LCA means suppose there is a state which is used by C4 & C3, then we should store the state in there LCA i.e. in C1. ( Also its a common sense that if there is a state which only C5 needs then we store it C5 itself okk. )
Now if there is a state which is used by C4 & C3, so we need to store it in C1, and we can directly pass it to C3, but to pass it to C4 we need to first pass it to C2 and C2 will pass it to C4, this is **Prop Drilling**. Like C2 doesn't need the prop (state), its just drilling it down. In below example *setCount* is passed to *Buttons* via *Count* enven though *Count* is not using it.
![[Pasted image 20260111093041.png]]
**Prop Drilling** makes code very unstructured & un-managable, it has **nothing to do with *Re-rendering*.** 
![[Pasted image 20260111100752.png]]
![[Pasted image 20260111100804.png]]
![[Pasted image 20260111100917.png]]
**Context API**: This lets us fix the *prop drilling*. It lets us pass the state variables without drilling them down, it lets us *teleport* state variables. Its also not for making code more performant, its just make code look more cleaner. Good resourse to read: https://react.dev/learn/passing-data-deeply-with-context

![[Pasted image 20260111101346.png]]
![[Pasted image 20260111101502.png]]
If we are using the context api, we are pushing our state management outside the *core react components*. It means it is still doing prop drilling but internally, not in code/core components. So we might think like state has been teleported from C1 to C4 so C2 won't re-render, but thats not the case because as we said Context API is only for making code more cleaner & well structured, it has nothing to with re-rendering or performance. For optimize re-rendering & performance we use *State Management tools*.
![[Pasted image 20260111101849.png]]
e.g. Modified above code a bit (added one extra component)
![[Pasted image 20260111103927.png]]
![[Pasted image 20260111103702.png]]
- Define Context in another file, basically create a teleportor
![[Pasted image 20260111102940.png]]
- Wrap anyone who wants to use the teleported value inside a *Provider*, and give *value* that *state variable*.
![[Pasted image 20260111103329.png]]
- Now use/access that value using *useContext* hook
![[Pasted image 20260111103545.png]]
## State Management
![[Pasted image 20260111125520.png]]
#### State Management using Recoil
![[Pasted image 20260111125644.png]]
![[Pasted image 20260111130005.png]]
![[Pasted image 20260111130107.png]]
The benefit of this is, we can define the *atom (state)* outside the components, like in a different file, and whichever component need that state can directly access to it, and only that component gets re-rendered.
![[Pasted image 20260111130514.png]]
like here, C3 & C4 can directly access the count state variable, only C3 & C4 will re-renders.
![[Pasted image 20260111130722.png]]
![[Pasted image 20260111131636.png]]
docs: https://recoiljs.org/docs/introduction/getting-started
![[Pasted image 20260111131921.png]]
**key**: unique key uniquely identify the atom
**default**: default value of the variable
![[Pasted image 20260111132714.png]]
So we use *useState* hook to define state, and what does state has, it has two things, one is the value and other is function to set that value. 
e.g. `const [stateValue, setStateValue] = useState(initialValue);` 
So *recoil* basically gives us three things
1. **useRecoilState(atomName)**: basically same as useState, gives both value and set function
2. **useRecoilValue(atomName)**: gives only value (*stateValue*)
3. **useSetRecoilState(atomName)**: gives set function (*setStateValue*)
![[Pasted image 20260111133542.png]]
and only if we want value then:
![[Pasted image 20260111133414.png]]
Now finally like Context API, we need to wrap the components which going to use recoil inside *RecoilRoot* component
![[Pasted image 20260111134154.png]]
- Using *recoil* means its not like you should never use *useState*, *recoil* is used for **global** level states ( global state means states which creates your app ), if you know a state variable **needs to be created and used inside same component** then it makes no sense to create an *atom* for it. e.g. the input component below, it just reads the value inside the input box
![[Pasted image 20260111134819.png]]
Now if you see below code:
![[Pasted image 20260111140019.png]]
Here we took both *count & setCount*. But as *count* is getting updated in *Buttons* component, it will re-render the *Buttons* too (can check console log ), so we can modify the code like below:
![[Pasted image 20260111135919.png]]
Here we only took the *setCount* and when we are calling the *setCount* by passing the arrow function, it is automatically getting *count* and as *count* is not getting updated in *Buttons* component, they won't re-render.
**Selector**:
![[Pasted image 20260114212545.png]]
read selector documentation https://recoiljs.org/docs/basic-tutorial/selectors
1:02:15