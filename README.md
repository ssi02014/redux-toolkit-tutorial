# ๐ป Redux-toolkit-Tutorial

## ๐โโ๏ธ Start

- yarn create react-app (ํ๋ก์ ํธ ์ด๋ฆ) --template redux
- yarn add @reduxjs/toolkit redux-devtools-extension
- yarn add @types/react-redux //ํ์์คํฌ๋ฆฝํธ

<br />

## ๐จโ๐ป configureStore

- Redux Toolkit์๋ Redux ์ฝ๋๋ฅผ ๋จ์ํํ๋๋ฐ ๋์์ด ๋๋ ๋ช ๊ฐ์ง ๊ธฐ๋ฅ ์ค ์ฒซ ๋ฒ์ฌ๊ฐ `configureStore`์ด๋ค.
- ์ผ๋ฐ์ ์ผ๋ก createStore()๋ฅผ ํธ์ถํ๊ณ  root reducer ํจ์๋ฅผ ์ ๋ฌํ์ฌ redux store๋ฅผ ๊ตฌ์ฑํ๋ค.
- Redux Toolkit์ createStor()๋ฅผ ๋ํํ configureStore() ํจ์๋ฅผ ์ ๊ณตํ๊ณ  ์ด ํจ์๋ ๊ธฐ๋ณธ์ ์ผ๋ก createStore()๊ณผ ๋์ผํ ๊ธฐ๋ฅ์ ์ ๊ณตํ๋ค. ํ์ง๋ง configureStore()๋ store๋ฅผ ์์ฑํ๋ ๋จ๊ณ์์ ๋ช ๊ฐ์ง ์ ์ฉํ ๊ฐ๋ฐ ๋๊ตฌ๊ฐ ์ค์ ๋๋๋ก ํ๋ค.
- configureStore()๋ ์ฌ๋ฌ ๊ฐ์ ์ธ์ ๋์  ์ด๋ฆ์ด ์ง์ ๋ ํ๋์ object๋ฅผ ์ธ์๋ก ๋ฐ์ผ๋ฏ๋ก, reducer ํจ์๋ฅผ reducer๋ผ๋ ์ด๋ฆ์ผ๋ก ์ ๋ฌํด์ผ ํ๋ค.

```ts
// Before:
const store = createStore(counter);

// After:
const store = configureStore({
  reducer: counter,
});

// Example
const reducer = {
    ...contractsReducer,
    (...)
}

const rootReducer = combineReducers(reducer);
export const  persistedReducer = persistReducer(persistConfig, rootReducer);
export const store = configureStore({
  reducer: persistedReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: (...),
      },
    })
      .prepend()
      // prepend and concat calls can be chained
      .concat(middlewares),
  devTools: process.env.NODE_ENV !== "production",
});
```

<br />

## ๐จโ๐ป createAction

- createAction์ ์ก์ ํ์ ๋ฌธ์์ด์ ์ธ์๋ก ๋ฐ๊ณ , ํด๋น ํ์์ ์ฌ์ฉํ๋ ์ก์ ์์ฑ์ํจ์๋ฅผ ๋ฐํํ๋ค.

```js
// Before: ์ก์ type๊ณผ ์์ฑํจ์๋ฅผ ๋ชจ๋ ์์ฑ
const INCREMENT = "INCREMENT";

function incrementOriginal() {
  return { type: INCREMENT };
}

console.log(incrementOriginal()); // {type: "INCREMENT"}

// After: createAction ์ฌ์ฉ
const incrementNew = createAction("INCREMENT");

console.log(incrementNew()); // {type: "INCREMENT"}
```

- createAction์ ์ฌ์ฉํ์ฌ counter ์์  ๋จ์ํ

```js
const increment = createAction("INCREMENT");
const decrement = createAction("DECREMENT");

function counter(state = 0, action) {
  switch (action.type) {
    case increment.type:
      return state + 1;
    case decrement.type:
      return state - 1;
    default:
      return state;
  }
}
```

<br />

## ๐จโ๐ป createReducer

- if๋ฌธ๊ณผ ๋ฐ๋ณต๋ฌธ์ ํฌํจํ์ฌ reducer์์ ์ํ๋ ์กฐ๊ฑด ๋ผ๋ฆฌ๋ฅผ ์ฌ์ฉํ  ์ ์์ง๋ง, ๊ฐ์ฅ ์ผ๋ฐ์ ์ธ ๋ฐฉ๋ฒ์ action.type ํ๋๋ฅผ ํ์ธํ๊ณ  ๊ฐ ์ ํ์ ๋ํด ์ ์ ํ ๋ก์ง์ ์ํํ๋ ๊ฒ์ด๋ค.
- reducer๋ ์ด๊ธฐ ์ํ๊ฐ์ ์ ๊ณตํ๊ณ , ํ์ฌ ์ก์๊ณผ ๊ด๊ณ์๋ ์ํ๋ ๊ทธ๋๋ก ๋ฐํํ๋ค.
- Redux Toolkit์๋ `lookup Table` ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํ์ฌ reducer๋ฅผ ์์ฑํ  ์ ์๋ createReducer()๊ฐ ์๋ค.
- createReducer() ๊ฐ์ฒด์ ๊ฐ ํค๋ redux์ ์ก์ type ๋ฌธ์์ด์ด๋ฉฐ ๊ฐ์ reducerํจ์์ด๋ค.
- ์ก์ type ๋ฌธ์์ด์ ํค๋ก ์ฌ์ฉํด์ผ ํ๋ฏ๋ก `ES6 object computer ์์ฑ` ๊ตฌ๋ฌธ์ ์ฌ์ฉํ์ฌ type๋ฌธ์์ด ๋ณ์๋ก ํค๋ฅผ ์์ฑํ  ์ ์๋ค.
- computed ์์ฑ ๊ตฌ๋ฌธ์ ๋ด๋ถ์ ์๋ ๋ชจ๋  ๋ณ์์ ๋ํด `toString()`์ ํธ์ถํ๋ฏ๋ก `.type`ํ๋์์ด ์ง์  ์ก์ ์์ฑ์ ํจ์๋ฅผ ์ฌ์ฉํ  ์ ์์ต๋๋ค.

```js
const increment = createAction("INCREMENT");
const decrement = createAction("DECREMENT");

const counter = createReducer(0, {
  [increment]: (state) => state + 1,
  [decrement]: (state) => state - 1,
});
```

<br />

## ๐จโ๐ป createSlice

- ์์ ๋ด์ฉ์ผ๋ก๋ ๋์์ง ์์ง๋ง, createSlice๋ก ๋ ํฐ ๋ณํ๋ฅผ ์ค ์ ์๋ค.
- createSlice ํจ์๋ ๊ฐ์ฒด์ reducer ํจ์๋ค์ ์ ๊ณตํ  ์ ์๊ณ  ์ด๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ์ก์ ํ์ ๋ฌธ์์ด๊ณผ ์ก์ ์์ฑ์ ํจ์๋ฅผ ์๋์ผ๋ก ์์ฑํ๋ค.
- createSlice๋ ์์ฑ๋ reducer ํจ์๋ฅผ reducer๋ผ๋ ํ๋๋ฅผ ํฌํจํ๋ `slice`๊ฐ์ฒด์ `actions`๋ผ๋ ๊ฐ์ฒด ๋ด๋ถ์์ ์์ฑ๋ ์ก์ ์์ฑํจ์๋ฅผ ๋ฐํํ๋ค.

```js
const counterSlice = createSlice({
  name: "counter",
  initialState: 0,
  reducers: {
    increment: (state) => state + 1,
    decrement: (state) => state - 1,
  },
});

const store = configureStore({
  reducer: counterSlice.reducer,
});
```

- ๋๋ถ๋ถ์ ๊ฒฝ์ฐ, ES6 ๋์คํธ๋ญ์ฒ๋ง ๊ตฌ๋ฌธ์ ์ด์ฉํ์ฌ ์ก์ ์์ฑ์ ํจ์์ reducer๋ฅผ ๋ณ์๋ก ์ฌ์ฉํ๊ธฐ๋ฅผ ์ํ๋ค.

```js
export const { increment, decrement } = counterSlice.actions;
```

<br />
<hr />

## ๐จโ๐ป createSlice(์ค๊ธ)

- createSlice ์ต์
  - name: ์์ฑ ๋ action types๋ฅผ ์์ฑํ๊ธฐ ์ํด ์ฌ์ฉ๋๋ prefix
  - initialState: reducer์ ์ด๊ธฐ ์ํ
  - reducers: key๋ action type ๋ฌธ์์ด์ด ๋๊ณ  ํจ์๋ ํด๋น ์ก์์ด dispatch๋  ๋ ์คํ๋  reducer์ด๋ค.
- ์๋ก, `todos/addTodo`์ก์์ด dispatch๋  ๋ addTodo Reducer๊ฐ ์ํ๋๋ค.
- createSlice์ createReducer๋ `immer library`์ `produce`๋ก ๋ํํ๋ค. ์ด๊ฒ์ ์ด ํจ์๋ฅผ ์ฌ์ฉํ๋ ๊ฐ๋ฐ์๋ ๋ฆฌ๋์ ๋ด๋ถ์ ์ํ๋ฅผ `๋ณํํ๋` ์ฝ๋๋ฅผ ์์ฑํ  ์ ์์ผ๋ฉฐ, immer๋ ์ํ๋ฅผ ์์ ํ๊ฒ ๋ถ๋ณํ๊ฒ ๋ค๋ฃฐ ์ ์๋๋ก ์ฒ๋ฆฌํด์ค๋ค.(์ฆ, push ๊ฐ์ ๋ฉ์๋ ์ฌ์ฉ ๊ฐ๋ฅ)

<br />

- createSclie๋ ๋ค์๊ณผ ๊ฐ์ ๊ฐ์ฒด๋ฅผ ๋ฐํํ๋ค.

```
  {
  name: "todos",
  reducer: (state, action) => newState,
  actions: {
    addTodo: (payload) => ({type: "todos/addTodo", payload}),
    toggleTodo: (payload) => ({type: "todos/toggleTodo", payload})
  },
  caseReducers: {
    addTodo: (state, action) => newState,
    toggleTodo: (state, action) => newState,
  }
}
```

- ๊ฐ ๋ฆฌ๋์๋ง๋ค ์ ์ ํ action ์์ฑ์์ action type์ ์๋์ผ๋ก ์์ฑํ๋ฏ๋ก ์ง์  ์์ฑํ์ง ์์๋ ๋๋ค.

<br />

## ๐จโ๐ป createAsyncThunk

- `createAsyncThunk`๋ฅผ ์ ์ธํ๊ฒ ๋๋ฉด ์ฒซ ๋ฒ์งธ ํ๋ผ๋ฏธํฐ๋ก ์ ์ธํ ์ก์ ์ด๋ฆ์ pending, fulfilled, rejected์ ์ํ์ ๋ํ action์ ์๋์ผ๋ก ์์ฑํด์ฃผ๊ฒ ๋๋ค.
- AbortController๋ฅผ ์ง์ํ๊ธฐ ๋๋ฌธ์ thunk๋ฅผ ์ฌ์ฉํ์ฌ๋ api์ ๋ํ ์ทจ์ ์์์ด ๊ฐ๋ฅํ๋ค.

```js
const fetchTodo = createAsyncThunk(
  `todo/fetchTodo`, // ์ก์ ์ด๋ฆ์ ์ ์ํด ์ฃผ๋๋ก ํฉ๋๋ค.
  async (todoId, thunkAPI) => {
    // ๋น๋๊ธฐ ํธ์ถ ํจ์๋ฅผ ์ ์ํฉ๋๋ค.
    const response = await todoApi.fetchTodoInfo(todoId);
    return response.data;
  }
);

// fetchTodo.pending => todo/fetchTodo/pending
// fetchTodo.fulfilled  => todo/fetchTodo/fulfilled
// fetchTodo.rejected  => todo/fetchTodo/rejected
```

<br />
