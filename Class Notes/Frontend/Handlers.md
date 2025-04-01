+

``` JavaScript
import { useState } from "react";
function App() {
  const [myText, setMyText] = useState("Tu Nombre");
  const [myValue, setMyValue] = useState("");
  const handleInput = (e) => {
    console.log(e.target.value);
    setMyValue(e.target.value);
  };
  return (
    <div>
      <h1>Hola a todos</h1>
      <h2>{myValue}</h2>
      <input
        type="text"
        placeholder={myText}
        value={myValue}
        onChange={handleInput}
      />
    </div>
  );
}
export default App;
```

The handlers functions always work with the event. The reason these handlers use the structure `(e.target.value)` is because in the console the browser records it as a keyboardEvent (e)+target and within the target field we have the value input, which in this example is "p".

![Handler event recorded by the console](resources/images/HandlerInConsole.png)