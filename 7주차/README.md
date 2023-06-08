# 리덕스 예제로 보는 사용법

## 리덕스 기본 원리

`Action -> Dispatch -> Store -> View`

- view: 컴포넌트
- action: 어떠한 변화인지 정의하는 이름
- dispatch: store의 state를 바꾸는 함수

1. view에서 action을 인자로 넣은 dispatch 함수를 호출
2. 해당 action의 정의된 reducer 로직에 따라 store의 state가 변화
3. 변화된 state를 바라보고 있는 view는 state를 업데이트 시킴

## 기본 세팅

1. [store 생성](#1-store-생성)
2. [app에 store 주입](#2-app에-store-주입)
3. [세부 reducer 정의](#3-세부-reducer-정의)

### 1. store 생성

`/src/store/store.ts`

```js
import { configureStore } from "@reduxjs/toolkit";
import { dashboardReducer } from "./dashboard/dashboard";
import { campaignReportReducer } from "./dashboard/campaignReport";

// store 생성
export const store = configureStore({
  reducer: {
    // 세부 reducers
    dashboard: dashboardReducer,
    campaignReport: campaignReportReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### 2. app에 store 주입

`/src/main.tsx`

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import { store } from './store/store';
import { Provider } from 'react-redux';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
    <React.StrictMode>
        <Provider store={store}>
            <App />
        </Provider>
    </React.StrictMode>
);
```

### 3. 세부 reducer 정의

`/src/store/dashboard/dashboard.ts`

```js
import { createSlice } from "@reduxjs/toolkit";
import { RootState } from "store/store";

const initialState = {
  dashboardData: null,
  date: null,
};

// initialState와 slice name을 받아 reducer와 상태에 해당하는 action 생성자와 action type을 자동으로 생성
const slice = createSlice({
  name: "dashboard",
  initialState,
  reducers: {
    // 2. 해당 action의 정의된 reducer 로직에 따라 store의 state가 변화
    setDashboardData(state, action) {
      state.dashboardData = action.payload;
    },
    setDatePickerDate(state, action) {
      state.date = action.payload;
    },
  },
});

// action export
// dispatch(setDashboardData(data))
export const { setDashboardData, setDatePickerDate } = slice.actions;

// export reducer
export const dashboardReducer = slice.reducer;

// selector
export const dashboardDataSelector = (state: RootState) =>
  state.dashboard.dashboardData;
export const dashboardDateSelector = (state: RootState) => state.dashboard.date;
```

## view에서 스토어 사용방법

### 상태 변화를 위한 dispatch

view에서 상태값이 변경되어 store에 업로드 해야하는 상황

```js
// 1. view에서 action을 인자로 넣은 dispatch 함수를 호출
const dispatch = useAppDispatch();
dispatch(setDashboardData(data));
```
#### `useAppDispatch`란?
dispatch - Redux 스토어 기능, 상태 변경을 트리거 하는 방법
useAppDispatch - 타입이 지정된 dispatch를 사용하기 위함

### store의 state를 구독

```js
// 3. 변화된 state를 바라보고 있는 view는 state를 업데이트 시킴
const dashboardData = useAppSelector(dashboardDataSelector);
```

# 참조

- [리덕스 잘 쓰고 계시나요?](https://ridicorp.com/story/how-to-use-redux-in-ridi/)
- [react-redux 사용법](https://kyounghwan01.github.io/blog/React/redux/redux-basic/#react-redux-%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8)
- [configureStore 공식문서](https://redux-toolkit.js.org/api/configureStore)
- [createSlice 공식문서](https://redux-toolkit.js.org/api/createSlice)
- [React - configureStore를 사용한 스토어 생성](https://phsun102.tistory.com/103)
- [dispatch 공식문서](https://react-redux.js.org/using-react-redux/connect-mapdispatch)
