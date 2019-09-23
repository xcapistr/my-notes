[< back](../README.md)

[< react](./react-tutorial.md)
# REDUX
![](../img/tenor.gif)

 [https://redux.js.org](https://redux.js.org/) 
management stavu, ktory je ulozeny v centralnom store
`npm install --save redux`

`const redux = require('redux');`

**1.** najskor treba vytvorit **reducer** - 2 argumenty - state (sem priradim pociatocny stav) a action
```js
const rootReducer = (state = initialState, action) => {
  return state;
};
```

**2.** potom vytvorim **store** (najlepsie v index.js, vid bod 6)
`const store = redux.createStore(rootReducer)`


k stavu sa potom dostanem cez store
`store.getState();`

**3.** definovanie **dispatching action** (povinny argument type a hodnota je uppercase, dalsie argumenty su additional info) - nemusim ju rovno dispatchovat, funkcie si vacsinou preddefinujem v mapDispatchToProps, nasledujuci priklad je priame dispatchovanie(vykonanie) akcie:
`store.dispatch({ type: 'ADD_COUNTER', value: 10 });`

**4.** pridanie funkcii do reducera
```js
const rootReducer = (state = initialState, action) => {
 if (action.type === "INC_COUNTER") {
   const newState = Object.assign({}, state);
   newState.counter = state.counter + 1;
   returnnewState;
  }
 return state;
};
```

kratsi zapis
```js
constrootReducer= (state=initialState,action)=>{
 if(action.type==="INC_COUNTER") {
  return{
    ...state,
    counter: state.counter+ 1
   };
  }
 returnstate;
};
```
!!!state niekdy priamo nemenime, vzdy treba urobit kopiu a tu vratit

**5. Subscription** - definujem hned po store - reaguje na kazdu zmenu/akciu, napr:
```js
store.subscribe(() => {
 console.log("[Subscription]", store.getState());
});
```

-store vytvorim v index.js, reducer sem importujem
```js
import { createStore } from 'redux';
const store = createStore(reducer);
```

-reducery vytvaram v samostatnych suboroch, root reducer:./src/store/reducer.js
-v rootReduceri inicializujem aj stav (initialState)

**6.** pripojenie reduxu do reactu - potrebujem package **react-redux**
`npm install --save react-redux`

potom v index.js importujem **provider** a zabalim do neho app komponent
```js
import { Provider} from "react-redux";
ReactDOM.render(<Provider store={store}><App /><_Provider>, document....
```
**pripojenie store do reactu** -v jednotlivych komponentoch potom nastavujem pripojenie:
`import { connect } from "react-redux"`

pred exportom komponentu musim vyriesit namapovanie stavu a dispatch do props,
connect nie je HOC ale funkcia ktora vracia HOC, exportujemnasledovne:
```js
const mapStateToProps = state => {
 return {
  ctr: state.counter
 }
}

const mapDispatchToProps = dispatch => {
 return {
  onIncrementCounter: () => dispatch({ type: "INCREMENT" })
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```
-namapovanim state do props (mimo triedu) si vyberiem atributy stavu a budem ich pouzivat ako props, takze uz sa nebudem odkazovat stylom `state.counter` ale `props.ctr`, zaroven uz nepotrebujem uchovavat state v komponente

!ked pridavam prvok do pola pomocou `push()` menim povodne pole, ak pouzijem `concat()` vytaram novu kopiu pola, pomocou `filter()` tiez vytvaram kopiu

**Outsourcing typov akcii v reduxe**
actions do samostatneho suboru ./src/store/actions.js - tu exportujem konstanty s nazvami akcii aby v buducnosti nedochadzalo ku preklepom:

`export const NEJAKA_AKCIA = 'NEJAKA_AKCIA'`

potom do reducer.js importujem
```js
import * as actionTypes from './actions'

case actionTypes.NEJAKA_AKCIA....
```

**Kombinovanie reducerov**
v index.js
```js
import { combineReducers } from 'redux';

const rootReducer = combineReducers({
 ctr: counterReducer,
 res: resultReducer
});
```

potom pristupujem kstate s jednym zanorenim naviac: `state.ctr.counter`
-nevyhoda - kazdy reducer ma svoj stav, takze nemozem v jednom reduceri nahliadnut do stavu druheho

**Typy stavu**
nie vsetok state musime managovat pomocou reduxu
-local UI state - napr. show/hide modal - netreba redux
-persistentny stav - napr. uzivatelia, orders,... - vacsina stavu je na servri (DB), relevantna cast moze byt manageovana reduxom
-client state - napr. is Auth? filters set by user - definitivne manageovat reduxom

**REDUX ADVANCED**
-zapojenie middleware do projektu (medzi dispach akcie a reducery - ked chcem urobit nieco z akciou pred tym ako dojde do reducera (napr. log))
-middleware - funkcie/kod ktore hooknem do procesu a ktore sa potom vykonaju ako sucast procesu bez toho aby sa zastavil

v index.js (tam kde vytvaram store)
```js
import { applyMiddleware compose } from "redux";

const logger = store => {
 return next => {
 return action => {
  console.log("[Midleware] Dispatching", action);
  const result = next(action);
  console.log("[Middleware] next state", store.getState());
  return result;
  };
 };
};

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(
 rootReducer,
 composeEnhancers(applyMiddleware(logger))
);
```

-toto bude vypisovat logy vzdy, ked sa dispatchne nejaka akcia

**redux devtool for chrome**
 [https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en) 
 [https://github.com/zalmoxisus/redux-devtools-extension](https://github.com/zalmoxisus/redux-devtools-extension) 
-zapojenie je o nieco zlozitejsie ked pouzivame middleware
-prave na to pouzivame **composeEnhancers**
-potom mozem vo vyvojarskych nastrojoch sledovat postupne zmeny stavu a prechadzat medzi nimi

**Asynchronne funkcie** (vracajuce propmisy) - nejdu len tak pridat do reducera
**async kod** v reduceri volame cez **action creator** (definujeme v ./src/store/actions/actions.js)
definovanie akcie:
```js
export const increment = () => {
 return {
  type: INCREMENT
 }
}
```

-konvencia je pomenovat funkciu rovnako ako typ ale s cammelCase

tam kde ju dispatchujem potompridam import
`import { increment } from ".._.._store_actions_actions";`

v `mapDsipatchToProps` potom zapisem:
`onIncrementCounter: () => dispatch(increment()),`

vacsinou je akcii viac takze zapisem:
```js
import * as actionCreators from "../../store/actions/actions";

onIncrementCounter: () => dispatch(actionCreators.increment()),...
```

**Asynchronne volanie**
pre asynchronne spravanie pridam middleware (hook medzia akciou a reducerom), pouzijem kniznicu **redux-thunk**
`npm install --save redux-thunk`

**v index.js**
```js
import thunk from "redux-thunk";
const store = createStore(
 rootReducer,
 composeEnhancers(applyMiddleware(logger, **thunk**))
);
```
-teraz mozem v actions.js definovat akciu (`storeResult`), ktora bue asynchronne volat inu akciu(`saveResult`)
```js
export const storeResult = res => {
 console.log(res);
 return dispatch => {
  setTimeout(() => {
   dispatch(**saveResult**(res));
    }, 2000);
  };
};
```

**!** Kde umiestnit **logicke operacie**? Do **Action Creatora** alebo do **Reducera**?
**action creator** - tu moze byt asynchronny kod, nemal by sa tu prilis pripravovat update stavu
**reducer** - len synchronny kod, zakladny redux koncept - update stavu

**getState v action creatore**
**action creator**- redux-thunk moze mat v creatore dalsi atribut - `getState`
takto mozme pristupit k stavu hned pred tym nez sa asynchronne dispatchne nejaka akcia:
```js
export const storeResult = res => {
  console.log(res);
 return (dispatch, **getState**) => {
  setTimeout(() => {
   const oldCounter = **getState()**.ctr.counter;
   console.log("old counter:", oldCounter);
   dispatch(saveResult(res));
  }, 2000);
 };
};
```
**pridanie utility funkcii**
-rozne funkcie na ulahcenie prace/zprehladnenie kodu
-v zlozke store -> utility.js
-napr. utilita pre update objektu:
```js
export const updateObject = (oldObject, updatedValues) => {
return {
  ...oldObject,
  ...updatedValues
 };
};
```
po starom som v reduceri zapisal:
```js
case actionTypes.DECREMENT:
return {
  ...state,
 counter: state.counter - 1
};
```
po importe utility `updateObject` mozem zapisat:
```js
case actionTypes.DECREMENT:
 return updateObject(state, { counter: state.counter - 1 });
```
**zostihlenie switch-case v reduceri**
-vpodstate tu ide len o to aby bol na kazdy case len 1 riadok kodu (return)
-vsetko naviac dame do samostatnej funkcie v tom istom subore a pomenujeme ju tak ako sa vola akcia v danom case (akurat v cammelCase)

**Adresarova struktura ./store**
./store
  ./actions
   ./actionTypes.js
   ./index.js
   ./volaco1.js
   ./volaco2.js
  ./reducers
   ./volaco1.js
   ./volaco2.js

**--koniec reduxu**-dostudovat dokumentaciu, hlavne [https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns](https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns) 