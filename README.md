## 리액트 hooks

## 1. useState
``` javascript
import { useState } from "react";

function App() {
const [item,setItem] = useState(1)
const 증가버튼 = ()=> setItem(item + 1)
const 감소버튼 = ()=> setItem(item - 1)
return (

<div className="App">
<h1>Hello {item}</h1>
<button onClick={증가버튼}>증가</button>
<button onClick={감소버튼}>감소</button>
</div>
);
}

export default App
```
## 2. useInput
``` javascript
import { useState } from "react";
import "./styles.css";

const useInput = (initialValue, validator) => {
const [value, setValue] = useState(initialValue);
const onChange = (event) => {
const {
target: { value }
} = event;
let willUpdate = true;
if (typeof validator === "function") {
willUpdate = validator(value);
}
if (willUpdate) {
setValue(value);
}
};
return { value, onChange };
};

function App() {
const maxLen = (value) => value.length <= 10;
const cut = (value) => !value.includes("@")
const name = useInput("Mr.", cut);
return (

<div className="App">
<h1>Hello </h1>
<input placeholder="Name" {...name} />
</div>
);
}

export default App;
```
## 3. useTabs
``` javascript
import { useState } from "react";
import "./styles.css";

const content = [
{
tab: "Section 1",
content: "I`m the content of the Section 1"
},
{
tab: "Section 2",
content: "I`m the content of the Section 2"
}
];

const useTabs = (initialTab, allTabs) => {
if (!allTabs || !Array.isArray(allTabs)) {
return;
}
const [currentIndex, setcurrentIndex] = useState(initialTab);
return {
currentItem: allTabs[currentIndex],
changeItem: setcurrentIndex
};
};
function App() {
const { currentItem,changeItem } = useTabs(0, content);
return (

<div className="App">
{content.map((section, index) => (
<button onClick={()=>changeItem(index)}>{section.tab}</button>
))}
<div>{currentItem.content}</div>
</div>
);
}

export default App;
```
---

## 1.useEffect

## 2.useTitle
``` javascript
import React, { useState, useEffect } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useTitle = (initialTitle) => {
const [title, setTitle] = useState(initialTitle);
const updateTitle = () => {
const htmlTitle = document.querySelector("title");
htmlTitle.innerText = title;
};
useEffect(updateTitle, [title]);
return setTitle;
};

function App() {
const titleUpdater = useTitle("Loading...");
setTimeout(() => titleUpdater("home"), 5000);
return <div className="App"></div>;
}

export default App;
```
## 3. useClick
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useClick = (onClick) => {
if (typeof onClick !== "function") {
return
}
const element = useRef();
useEffect(() => {
if (element.current) {
element.current.addEventListener("click", onClick);
}
return () => {
if (element.current) {
element.current.removeEventListener("click", onClick);
}
};
}, []);
return element;
};

function App() {
const sayHello = () => console.log("say hello");
const title = useClick(sayHello);
return (

<div className="App">
<h1 ref={title}>Hi</h1>
</div>
);
}

export default App;
```
## 4.useConfirm
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useConfirm = (message = "", callback, rejection) => {
const confirmAction = () => {
if (confirm(message)) {
callback();
} else {
rejection();
}
};
return confirmAction;
};

function App() {
const deleteWorld = () => console.log("성공");
const abort = () => console.log("실패");
const confirmDelte = useConfirm("Are you sure", deleteWorld, abort);
return (

<div className="App">
<button onClick={confirmDelte}>Delete</button>
</div>
);
}

export default App;
```
## 5.usePreventLeave
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const usePreventLeave = () => {
const listener = (event) => {
event.preventDefault();
event.returnValue = "";
};
const enablePrevent = () => window.addEventListener("beforeunload", listener);
const disablePrevent = () =>
window.removeEventListener("beforeunload", listener);
return { enablePrevent, disablePrevent };
};

function App() {
const { enablePrevent, disablePrevent } = usePreventLeave();
return (

<div className="App">
<button onClick={enablePrevent}>Protecnt</button>
<button onClick={disablePrevent}>Unprotecnt</button>
</div>
);
}

export default App;
```
## 6.useBeforeLeave
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useBeforeLeave = (onBefore) => {
const handle = (event) => {
const {clientY} = event
if (clientY <=0) {
onBefore()
}

}
useEffect(()=> {
document.addEventListener("mouseleave",handle)
return () => document.removeEventListener("mouseleave",handle)
}, [])
}

function App() {
const begForLife = () => alert("ㄴㄴ")
useBeforeLeave(begForLife)
return (

<div className="App">
<h1>Hello</h1>
</div>
);
}

export default App;
```
## 7.useFadeln
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useFadeIn = (duration=1, delay = 0) => {
const element = useRef()
useEffect(()=> {
if(element.current) {
const {current} = element
current.style.transition = `opacity ${duration}s ease-in-out ${delay}s`
current.style.opacity = 1
}
},[])
return {ref: element, style: {opacity:0}}

}

function App() {
const fadeInH1 = useFadeIn(2,3)
const fadeInP = useFadeIn(5,10)
return (

<div className="App">
<h1 {...fadeInH1} style={{opacity:0}}> Hello</h1>
<p {...fadeInP}>lalalalalalal</p>
</div>
);
}

export default App;
```
## 8.useNetwork
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useNetwork = onChange => {
const [status,setStatus] = useState(navigator.onLine)
const handleChange = () => {
setStatus(navigator.onLine)
}
useEffect(()=>{
window.addEventListener('online',handleChange)
window.addEventListener('offline',handleChange)

},[])
return status
}

function App() {
const handleNetworkChange = (online) => {
console.log(online? "online" : "offline")
}
const onLine = useNetwork(handleNetworkChange)
return (

<div className="App">
<h1>{onLine ? "Online" : "Offline"}</h1>
</div>
);
}

export default App;
```
## 9.useScroll
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useScroll = () => {
const [state, setState] = useState({
x: 0,
y: 0
});
const onScroll = () => {
setState({y:window.scrollY, x:window.scrollX});
};
useEffect(() => {
window.addEventListener("scroll", onScroll);
return () => window.removeEventListener("scroll", onScroll);
}, []);
return state;
};

export default function App() {
const { y } = useScroll();
return (
<div className="App" style={{ height: "1000vh" }}>
<h1 style={{ position: "fixed", color: y > 300 ? "red" : "blue" }}>Hi</h1>
</div>
);
}
```
## 10.useFullscreen
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useFullscreen = () => {
const element = useRef();
const triggerFull = () => {
if (element.current) {
element.current.requestFullscreen();
}
};
const exitFull = () => {
document.exitFullscreen()
}
return { element, triggerFull,exitFull };
};

export default function App() {
const { element, triggerFull } = useFullscreen();
return (
<div className="App" style={{ height: "1000vh" }}>
<img
        ref={element}
        img
        src="https://i.ibb.co/R6RwNxx/grape.jpg"
        alt="grape"
        width="250"
      />
<button onClick={triggerFull}>풀스크린 버튼</button>
</div>
);
}
```
## 11.useNotification
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";

const useNotification = (title, options) => {
if (!("Notification" in window)) {
return;
}
const fireNotif = () => {
if (Notification.permission !== "granted") {
Notification.requestPermission().then((permission) => {
if (permission === "granted") {
new Notification(title, options);
} else {
return;
}
});
} else {
new Notification(title, options);
}
};
return fireNotif;
};

export default function App() {
const triggerNotif = useNotification("안녕");
return (
<div className="App" style={{ height: "1000vh" }}>
<button onClick={triggerNotif}>클릭</button>
</div>
);
}
```
## 12.useAxios
``` javascript
import React, { useState, useEffect, useRef } from "react";
import ReactDom from "react-dom";
import "./styles.css";
import useAxios from "./useAxios";

export default function App() {
const { loading, data, error,refetch } = useAxios({
url: "https://yts.mx/api/v2/list_movies.json"
});
return (
<div className="App" style={{ height: "1000vh" }}>
<h1>{data && data.status}</h1>
<h2>{loading && "Loading"}</h2>
<button onClick={refetch}>클릭</button>
</div>
);
}

import defaultAxios from "axios";
import { useEffect, useState } from "react";

const useAxios = (opts, axiosInstance = defaultAxios) => {
const [state, setState] = useState({
loading: true,
error: null,
data: null
});
const [trigger, setTrigger] = useState(0)
if (!opts.url) {
return;
}
const refetch = ()=>{
setState({
...state,
loading:true
})
setTrigger(Date.now())
}
useEffect(() => {
axiosInstance(opts)
.then((data) => {
setState({
...state,
loading: false,
data
});
})
.catch((error) => {
setState({ ...state, loading: false, error });
});
}, [trigger]);
return {...state};
};

export default useAxios;
```
