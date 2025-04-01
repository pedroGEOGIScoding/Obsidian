[Axios official docs](https://axios-http.com/docs/intro)
Axios is a **JavaScript library** used for making **HTTP requests** in React applications. It is a promise-based HTTP client for node.js. It is **isomorphic** as it can run in the browser and nodejs with the same codebase.

It provides an easy-to-use and **consistent API for performing asynchronous operations**, such as **fetching data** from an API.

To install Axios using npm, run the following command in your terminal:
```JavaScript
npm install axios
```

The key words such as async, await and promise are not automatic, so that always apply a try and catch as we do not know the response:

```JavaScript
const fetchUsers = async () => {
try {
const response = await axios.get("/appusers");
setUsers(response.data);
} catch (error) {
console.error("Error fetching users:", error);
}
};
```