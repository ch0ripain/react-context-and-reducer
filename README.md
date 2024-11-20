# 🚀 A Quick Introduction to React Context and Reducer 🚀
## 🌐 Context 🌐
A context in React is a common way to make the state globally on all your app.
First you need to create your context. For that you are going to use a function provided by React ➡️ <code>createContext</code> 
```javascript
import { createContext } from "react";
export const CartContext = createContext({
  items: [],
  addItemToCart: () => {},
  updateCartItemQuantity: () => {},
});
```
As you can see above, we must import it to be available to use it.
<code>createContext</code> can receive any JS expression like <code>[], {}, 5, 'context'</code>
In this case we are defining a object with some properties with void values. This is a commonly good practice for have our context properties defined and also have autocompletation on our code.
A standard way to make our context usable is to make it a wrapper component using the <code>Context.Provider</code> property.

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
Last we need to wrap our main component so all the childs and the main component i guess will have access to our context.
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
As we can see above
<code>shoppingCartState</code> it might be only treaty as a read-only value.
<code>shoppingCartDistpach</code> is the dispatcher update function which we have to use to pass the new state changes and the type of change.
<code>useReducer()</code> receive as a first argument the function with all the logic to update the state and as a second argument the inital state value
As you can see useReducer is a hook similar to useState but in this hook we have to handle all the state update in a separated function.
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
<code>shoppingCartReducer</code> receive 2 parameters. 
1- state: the latest state at the moment we call this function
2- action: commonly a object with some key-values like <code>{type: 'ADD_ITEM', payload: id}</code>

## 🌐 Using context 🌐
For use context we need to import two things
```javascript
import { useContext } from "react";
import { CartContext } from "../store/shopping-cart-context";
```
Then we can destructure the context we need from the context object we created 
```javascript
const { items, updateCartItemQuantity } = useContext(CartContext);
```
And finally we just use it as it is supposed to be used
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
With all the above we've been implemented globally state using context avoiding prop drilling and separating the state updating logic with reducer making a code more leaner and easy to mantain
---
<p align="center">🌟 This project is a practice exercise I learned from the <a href='https://www.udemy.com/course/react-the-complete-guide-incl-redux/?couponCode=ST7MT110524'>Academind's React Course</a> 🌟</p>
<p align="center">🐸 I hope this README helps you in some way! 🐸</p>
