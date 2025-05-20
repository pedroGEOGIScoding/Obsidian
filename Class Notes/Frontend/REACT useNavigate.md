React Router v6 introduces the useNavigate() hook, making it easier and more flexible to navigate between different pages in your app. It replaces the older useHistory() hook.

With the `useNavigate() hook`, you can:

- Go to a different page in your app.
- Navigate based on actions like button clicks.
- Send data or parameters when moving to another page.
### Syntax

```
const navigate = useNavigate();
navigate('/targetpath')
navigate('/path', { replace: true });
```
- ****navigate('/targetpath'):**** Navigates to the specified path (/targetpath).
- ****navigate('/path', { replace: true }):**** Navigates to /path and replaces the current entry in the history stack.
## How to Use useNavigate() for Navigation?

The useNavigate() hook in [React Router](https://www.geeksforgeeks.org/reactjs-router/) v6 provides a programmatic way to navigate between routes in your application. Here's a step-by-step guide on how to use it:

### Step 1: Install React Router

```
npm install react-router-dom@6
```
### Step 2: Import the useNavigate Hook

To begin, you need to import the useNavigate hook from react-router-dom.

```
import { useNavigate } from 'react-router-dom';
```

### Step 3: Call useNavigate() Inside Your Component

Inside your functional component, call useNavigate() to access the navigate function. This function will allow you to programmatically change the route.

```
const navigate = useNavigate();
```

### Step 4: Use navigate() to Change Routes

You can use the navigate function to navigate to a specific route. Here's the basic syntax:

```
navigate('/path');  // Navigates to '/path'
```

### Step 5: Using replace for One-Time Redirection

You can also use the `{ replace: true }` option to replace the current history entry. This is useful for cases like login redirects, where you don’t want users to be able to use the back button to return to the previous page.

```
navigate('/path', { replace: true });
```
### Step 6: Navigating Back and Forward

To navigate back in the browser’s history, use a negative number (e.g., -1), and to move forward, use a positive number (e.g., 1):

```
navigate(-1);  // Goes back one step in history
navigate(1);   // Goes forward one step in history
```

## Implementation of useNavigate() Hook

****Step 1:**** To start with, create a React application using the following command:

```
npx create-react-app demo
```

****Step 2:**** Move to the project directory.

```
cd demo
```

****Step 3:**** Install the latest version of react-router-dom in the React application by the following.

```
npm install react-router-dom@6
```

****Project Structure:****

![](https://media.geeksforgeeks.org/wp-content/uploads/20220121134912/newdemogfg.png)

The updated dependencies in ****package.json**** file.

```
"dependencies": {
    "@testing-library/jest-dom": "^5.17.0",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.19.0",
    "react-scripts": "5.0.1",
    "web-vitals": "^2.1.4"
},
```
```javascript
// Filename - App.js
import React from "react";
import {
    BrowserRouter,
    Routes,
    Route,
} from "react-router-dom";
import Home from "./components/Home";
import About from "./components/About";

function App() {
    return (
        <>
            <BrowserRouter>
                <Routes>
                    <Route
                        path="/"
                        element={<Home />}
                    />
                    <Route
                        path="/about"
                        element={<About />}
                    />
                </Routes>
            </BrowserRouter>
        </>
    );
}

export default App;
```
```javascript
// Filename - components/Home.js
import React from "react";
import { useNavigate } from "react-router-dom";
const Home = () => {
    const navigate = useNavigate();
    return (
        <>
            <h1 style={{ color: "green" }}>
                GeeksForGeeks
            </h1>
            <button onClick={() => navigate("/about")}>
                About
            </button>
        </>
    );
};
export default Home;
```
```javascript
// Filename - components/About.js
import React from "react";
import { useNavigate } from "react-router-dom";
const About = () => {
    const navigate = useNavigate();
    return (
        <>
            <h1 style={{ color: "green" }}>
                A Computer Science portal for geeks.
            </h1>
            <button onClick={() => navigate(-1)}>
                Go Back Home
            </button>
        </>
    );
};
export default About;
```
### Output

![ReactJS useNavigate() Hook ](https://media.geeksforgeeks.org/wp-content/uploads/20220131152523/gobackgfg.gif)

****In this example:****

- ****App.js****: Sets up routing with `BrowserRouter`, defining routes for `/` (Home) and `/about` (About).
- ****Home.js****: Displays "GeeksForGeeks" and a button to navigate to the `/about` page using `useNavigate()`.
- ****About.js****: Displays a message and a button to go back to the home page using `useNavigate(-1)`.
- ****Navigation Flow****: Clicking "About" takes you to `/about`, and "Go Back Home" takes you back to `/`.
- ****useNavigate()****: Used to navigate programmatically between routes in both `Home` and `About`.

## Best Practices for Using useNavigate() in ReactJS

- ****Avoid Navigation in Render Phase:**** Don’t trigger navigation during the component's render. Use effects (like useEffect) or conditional checks for more efficient navigation.
- ****Use { replace: true } for One-Time Redirects:**** For actions like login or logout, use { replace: true } to prevent users from going back to the previous page after the redirect.
- ****Pass State for Sharing Data:**** When navigating, pass state to share data across routes, making it easier to transfer information between components.

## When to Use useNavigate() vs. <Link>

- ****useNavigate():**** Best suited for programmatic navigation triggered by code, such as after form submissions, authentication events, or specific actions that aren't directly related to user interaction.
- Link or NavLink: Ideal for declarative navigation, where the route change is a part of the UI and driven by user interaction, like in menu items, buttons, or navigation bars.
### Conclusion

The useNavigate() hook is a powerful and essential tool for navigation in React applications using React Router v6. It simplifies the process of moving between pages, passing data, and controlling the browser history. By using `useNavigate()`, you can create dynamic and interactive applications with seamless navigation experiences.