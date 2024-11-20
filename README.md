# 🚀 Quick Introduction to React Context & Reducer 🚀
## 🌐 Context 🌐
In React, a Context is a common way to share state globally across your application. 
To get started, you first need to create the context. This can be done using a function provided by React ➡️ <code>createContext</code> 
```javascript
import { createContext } from "react";
export const CartContext = createContext({
  items: [],
  addItemToCart: () => {},
  updateCartItemQuantity: () => {},
});
```
As shown above, we need to import <code>createContext</code> to make it available for use.
The createContext function can accept any JavaScript expression, such as <code>[], {}, 5, or 'context'</code>

In this example, we define an object with some properties initialized to undefined values. This is a common best practice as it allows us to clearly define the properties of our context while enabling code autocompletion in our editor.

A standard approach to make the context usable is to wrap it in a component using the <code>Context.Provider</code> property.

```javascript
export default function CartContextProvider({ children }) {
  const contextValue = {
    items: shoppingCartState.items,
    addItemToCart: handleAddItemToCart,
    updateCartItemQuantity: handleUpdateCartItemQuantity,
  };
 return (
    <CartContext.Provider value={contextValue}>{children}</CartContext.Provider>
  );
}
```
Finally, we need to wrap our main component with the context provider. This ensures that the main component and all its child components will have access to the context.
```javascript
import CartContextProvider from "./store/shopping-cart-context.jsx";

function App() {
  return (
    <CartContextProvider>
      <Header />
      <Shop>
        {DUMMY_PRODUCTS.map((product) => (
          <li key={product.id}>
            <Product {...product} />
          </li>
        ))}
      </Shop>
    </CartContextProvider>
  );
}
```

## 🌀 Reducer 🌀
```javascript
const [shoppingCartState, shoppingCartDispatch] = useReducer(
    shoppingCartReducer,
    {
      items: [],
    }
  );
```
As shown above, the <code>shoppingCartState<code> should be treated as a read-only value.

On the other hand, <code>shoppingCartDispatch<code> is the dispatcher function that we use to pass the updated state and the type of change we want to perform.

The <code>useReducer()<code> hook takes two arguments:

-A function that contains all the logic for updating the state.
-The initial state value.
In essence, useReducer is a hook similar to useState, but all state updates are managed in a separate function, providing a more structured way to handle complex state logic.
```javascript
function shoppingCartReducer(state, action) {
  if (action.type === "ADD_ITEM") {
    const updatedItems = [...state.items];

    const existingCartItemIndex = updatedItems.findIndex(
      (cartItem) => cartItem.id === action.payload.id
    );
    //...
  }
}
```
<code>shoppingCartReducer</code> function takes two parameters:

- state ➡️ the most recent state when the reducer function is called.
- action ➡️ typically an object containing key-value pairs, such as <code>{type: 'ADD_ITEM', payload: id}</code>

## 🌐 Using context 🌐
To use a context, we need to import two things:
```javascript
import { useContext } from "react";
import { CartContext } from "../store/shopping-cart-context";
```
Next, we can destructure the specific context we need from the context object we previously created.

```javascript
const { items, updateCartItemQuantity } = useContext(CartContext);
```
Finally, we use the context as intended in our application.
```javascript
const totalPrice = items.reduce(
    (acc, item) => acc + item.price * item.quantity,
    0
  );
```
```javascript
 <ul id="cart-items">
          {items.map((item) => {
          //...
          <button onClick={() => updateCartItemQuantity(item.id, -1)}>
          //...
          }
</ul>
```
With all the above, we have implemented a global state using context, avoiding prop drilling and separating the state update logic using a reducer. 
This approach makes our code cleaner, more maintainable, and easier to understand.
> [!NOTE]
> Prop drilling is passing data through multiple layers of components via props. Context avoids this by providing a way to share data directly with any component, no matter how deep it is in the tree.

---
<p align="center">🌟 This project is a practice exercise I learned from the <a href='https://www.udemy.com/course/react-the-complete-guide-incl-redux/?couponCode=ST7MT110524'>Academind's React Course</a> 🌟</p>
<p align="center">🐸 I hope this README helps you in some way! 🐸</p>
