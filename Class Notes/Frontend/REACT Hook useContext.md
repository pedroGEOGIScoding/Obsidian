**The `useContext` hook** in React is a function that allows components to access values from a context directly, without having to pass props manually through every level of the component tree. This is especially useful for managing global state or shared data, such as user information, themes, or authentication status, across deeply nested components.

```javascript
const value = useContext(SomeContext)
```
## Why Use `useContext`?

- **Avoids Prop Drilling:**  
    Without context, you would have to pass data through every intermediate component as props, even if only the deeply nested component needs it. `useContext` eliminates this by allowing direct access to the context value wherever it’s needed.
- **Simplifies State Management:**  
    It enables centralized management of shared state, making your code cleaner and easier to maintain, especially in large applications.
- **Automatic Updates:**  
    Components that use `useContext` will automatically re-render when the context value changes
Context works similarly to props, but instead of providing data to a single child, it can be used to provide data to an entire subtree.

> [!IMPORTANT]
> 
> 
> Context can only send data down the tree (parent to subtree). It can be paired with callback functions to pass data back up.

### Example

The service is generated here:
```javascript
import instanceAxios from "./api";

const planeService = {
  getAllPlanes: async () => {
    try {
      const response = await instanceAxios.get(`/planes`);
      return response.data;
    } catch (error) {
      console.error("Error fetching planes:", error);
      throw error;
    }
  },

  getPaginatedPlanes : async (currentPage) => {
    try {
      const response = await instanceAxios.get(`/planes?page=${currentPage}`);
      return response.data;
    } catch (error) {
      console.error("Error fetching planes", error);
      throw error;
    }
  },

  createPlane: async (plane) => {
    try {
      const response = await instanceAxios.post(`/planes`, plane);
      return response.data;
    } catch (error) {
      console.error("Error creating plane:", error);
      throw error;
    }
  },

  updatePlane: async (id, plane) => {
    try {
      const response = await instanceAxios.put(`/planes/${id}`, plane);
      return response.data;
    } catch (error) {
      console.error("Error updating plane:", error);
      throw error;
    }
  },

  deletePlane: async (id) => {
    try {
      const response = await instanceAxios.delete(`/planes/${id}`);
      return response.data;
    } catch (error) {
      console.error("Error deleting plane:", error);
      throw error;
    }
  },
};

export default planeService;
```

here is in packed as services:
```javascript
import airportService from "./airportService";
import planeService from "./planeService";
import flightService from "./flightService";

const Services = {
  airport: airportService,
  plane: planeService,
  flight: flightService,
};

export default Services;
```

and finally used here to create the context:

```javascript
import React, { createContext, useContext } from "react";
import Services from "./services";

const AppServicesContext = createContext(Services);

export const useAppServices = () => {
  return useContext(AppServicesContext);  
};

export const AppServicesProvider = ({ children }) => {
  return (
    <AppServicesContext.Provider value={Services}>
      {children}
    </AppServicesContext.Provider>
  );
};

export default AppServicesContext;
```