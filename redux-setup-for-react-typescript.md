# Redux Setup for React-Typescript Project

## Installation

`npm i -S redux react-redux @types/react-redux @types/redux redux-thunk @types/redux-thunk`

## Reducers

1. Create a reducer file for the entity as shown below:

```javascript
export interface ProductState{
 products: Product[] 
}

const intialState: ProductReducerState{
 products: []
}

const productReducer = (state: ProductState, action:any) => {

return state;
}

export default productReducer
```
2. Create Global State

```javascript
 import { ProductState } from "./reducers/productReducer";

export default interface GlobalState {
	product: ProductState;
}

```

3. Create a Rootreducer to hold all the reducers

```javascript
import {combineReducers} from "redux";
import productReducer from './productReducer.ts';

const RootReducer = combineReducers({
  product: productReducer
});

export default RootReducer;
```

## Store Creation with the Reducer

```javascript
import {createStore, applyMiddleware} from "redux";
import ReduxThunk from 'redux-thunk';
import RootReducer from './RootReducer';

const store = createStore(RootReducer, applyMiddleware([ReduxThunk]));

export default store;
```

## Add store to the Application

```javascript
import Provider from 'react-redux';
import store from './store';
<Provider store={store}>
//Your App
</Provider>
```

## Create Action Types

```javascript
import { Action, ActionCreator, Dispatch } from "redux";
import { ThunkAction } from "redux-thunk";
import GlobalState from "../GlobalState";
import { Product } from "../reducers/productReducer";

export enum ProductActionTypes {
	FETCH_PRODUCTS_REQUESTED = "FETCH_PRODUCTS_REQUESTED",
	FETCH_PRODUCTS_SUCCESSFUL = "FETCH_PRODUCTS_SUCCESSFUL",
	FETCH_PRODUCTS_FAILED = "FETCH_PRODUCTS_FAILED"
}

export type fetchProductsFailedAction =
	Action<ProductActionTypes.FETCH_PRODUCTS_FAILED>;

export type fetchProductsRequestedAction =
	Action<ProductActionTypes.FETCH_PRODUCTS_REQUESTED>;

export interface fetchProductsSuccessfulAction
	extends Action<ProductActionTypes.FETCH_PRODUCTS_SUCCESSFUL> {
	productsList: Product[];
}

export type ProductActions =
	| fetchProductsSuccessfulAction
	| fetchProductsFailedAction
	| fetchProductsRequestedAction;

interface ProductDispatch {
	fetchProducts: ActionCreator<
		ThunkAction<
			Promise<fetchProductsSuccessfulAction>,
			GlobalState,
			null,
			fetchProductsSuccessfulAction
		>
	>;
}

const getProducts = () => {
	return new Promise((resolve, reject) => {
		resolve([{ name: "oppo1" }, { name: "oppo2" }]);
	});
};

export const ProductActionCreators: ProductDispatch = {
	fetchProducts: () => {
		return async (
			dispatch: Dispatch
		): Promise<fetchProductsSuccessfulAction> => {
			try {
				const products = await getProducts();
				const productsFetchSuccessfulAction: fetchProductsSuccessfulAction = {
					type: ProductActionTypes.FETCH_PRODUCTS_SUCCESSFUL,
					productsList: products as Product[]
				};
				return dispatch(productsFetchSuccessfulAction);
			} catch (error) {
				return Promise.reject(error);
			}
		};
	}
};


```

## Add the created action to reducers

```javascript
import { ProductActions, ProductActionTypes } from "../Actions/productActions";

export interface Product {
	name: string;
}

export interface ProductState {
	products: Product[];
}

const intialState: ProductState = {
	products: []
};

const productReducer = (
	state: ProductState = intialState,
	action: ProductActions
) => {
	switch (action.type) {
		case ProductActionTypes.FETCH_PRODUCTS_SUCCESSFUL:
			return {
				...state,
				products: [...state.products, ...action.productsList]
			};
		default:
			return state;
	}
};

export default productReducer;
```

## use the action creator to fetch the data 

```javascript
import React from "react";
import GlobalState from "../../../GlobalState";
import "./index.less";

import { useDispatch, useSelector } from "react-redux";
import { ProductActionCreators } from "../../../Actions/productActions";

export interface ContentProps {}

const Content: React.FC<ContentProps> = () => {
	const dispatch = useDispatch();
	React.useEffect(() => {
		console.log("Content");
	}, []);

	const { productsList } = useSelector((state: GlobalState) => {
		return {
			productsList: state.product.products
		};
	});

	const getProducts = async () => {
		try {
			await dispatch(ProductActionCreators.fetchProducts());
		} catch (error) {
			console.log(error);
		}
	};

	console.log(productsList);
	return (
		<div className='content'>
			Content
			<button onClick={getProducts}>Get Products</button>
		</div>
	);
};

export default Content;

```

