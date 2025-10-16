## DOM
![[Pasted image 20250723074455.png]]

When the **ECMAScript specification** was written, it focused **strictly on the core language features**—such as variables, arrays, functions, objects, loops, etc. It is a **language-oriented document**, and does **not include any browser-specific functionalities**. Functions like: `setTimeout()`, `setInterval()`, `fetch()`, the `document` Object (DOM), and many other Web APIs are **not part of the ECMAScript specification**. These features are provided by the **browser environment**, not JavaScript itself.
So, the **JavaScript you run in the browser** is a **combination of two things**:
1. **ECMAScript** – the core language defined by the spec
2. **Browser APIs** – provided by the browser (like Chrome, Firefox) to interact with the web page and perform actions like timers, network requests, DOM manipulation, etc.
In short, **JavaScript in the browser = ECMAScript + Web APIs provided by the browser**.

![[Pasted image 20250723083439.png]]

#### Debounce Requests
![[Pasted image 20251005130342.png]]
##### ToDo
![[Pasted image 20251005161953.png]]
