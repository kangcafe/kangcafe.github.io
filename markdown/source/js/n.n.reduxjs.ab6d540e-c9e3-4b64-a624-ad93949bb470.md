# Reduxjs

```js
(function webpackUniversalModuleDefinition(root, factory) {
  if (typeof exports === 'object' && typeof module === 'object')
    module.exports = factory();
  else if (typeof define === 'function' && define.amd)
    define([], factory);
  else if (typeof exports === 'object')
    exports["Redux"] = factory();
  else
    root["Redux"] = factory();
})(this, function () {

  return (function (modules) {
    var installedModules = {};

    function __webpack_require__(moduleId) {
      if (installedModules[moduleId])
        return installedModules[moduleId].exports;
      var module = installedModules[moduleId] = {
        exports: {},
        id     : moduleId,
        loaded : false
      };
      modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);
      module.loaded = true;
      return module.exports;
    }

    __webpack_require__.m = modules;
    __webpack_require__.c = installedModules;
    __webpack_require__.p = "";
    return __webpack_require__(0);
  })

  ([
    function (module, exports, __webpack_require__) {
      'use strict';
      exports.__esModule = true;
      exports.compose    = exports.applyMiddleware = exports.bindActionCreators = exports.combineReducers = exports.createStore = undefined;
      var _createStore         = __webpack_require__(2);
      var _createStore2        = _interopRequireDefault(_createStore);
      var _combineReducers     = __webpack_require__(7);
      var _combineReducers2    = _interopRequireDefault(_combineReducers);
      var _bindActionCreators  = __webpack_require__(6);
      var _bindActionCreators2 = _interopRequireDefault(_bindActionCreators);
      var _applyMiddleware     = __webpack_require__(5);
      var _applyMiddleware2    = _interopRequireDefault(_applyMiddleware);
      var _compose             = __webpack_require__(1);
      var _compose2            = _interopRequireDefault(_compose);
      var _warning             = __webpack_require__(3);
      var _warning2            = _interopRequireDefault(_warning);

      function _interopRequireDefault(obj) {
        return obj && obj.__esModule ? obj : {"default": obj};
      }

      function isCrushed() {
      }

      if (("development") !== 'production' && typeof isCrushed.name === 'string' && isCrushed.name !== 'isCrushed') {
        (0, _warning2["default"])('You are currently using minified code outside of NODE_ENV === \'production\'. ' + 'This means that you are running a slower development build of Redux. ' + 'You can use loose-envify (https://github.com/zertosh/loose-envify) for browserify ' + 'or DefinePlugin for webpack (http://stackoverflow.com/questions/30030031) ' + 'to ensure you have the correct code for your production build.');
      }
      exports.createStore        = _createStore2["default"];
      exports.combineReducers    = _combineReducers2["default"];
      exports.bindActionCreators = _bindActionCreators2["default"];
      exports.applyMiddleware    = _applyMiddleware2["default"];
      exports.compose            = _compose2["default"];
    },
    function (module, exports) {
      "use strict";
      exports.__esModule = true;
      exports["default"] = compose;
      function compose() {
        for (var _len = arguments.length, funcs = Array(_len), _key = 0; _key < _len; _key++) {
          funcs[_key] = arguments[_key];
        }
        if (funcs.length === 0) {
          return function (arg) {
            return arg;
          };
        } else {
          var _ret = function () {
            var last = funcs[funcs.length - 1];
            var rest = funcs.slice(0, -1);
            return {
              v: function v() {
                return rest.reduceRight(function (composed, f) {
                  return f(composed);
                }, last.apply(undefined, arguments));
              }
            };
          }();
          if (typeof _ret === "object") return _ret.v;
        }
      }
    },
    function (module, exports, __webpack_require__) {
      'use strict';
      exports.__esModule     = true;
      exports.ActionTypes    = undefined;
      exports["default"]     = createStore;
      var _isPlainObject     = __webpack_require__(4);
      var _isPlainObject2    = _interopRequireDefault(_isPlainObject);
      var _symbolObservable  = __webpack_require__(11);
      var _symbolObservable2 = _interopRequireDefault(_symbolObservable);

      function _interopRequireDefault(obj) {
        return obj && obj.__esModule ? obj : {"default": obj};
      }

      var ActionTypes = exports.ActionTypes = {
        INIT: '@@redux/INIT'
      };

      function createStore(reducer, initialState, enhancer) {
        var _ref2;
        if (typeof initialState === 'function' && typeof enhancer === 'undefined') {
          enhancer     = initialState;
          initialState = undefined;
        }
        if (typeof enhancer !== 'undefined') {
          if (typeof enhancer !== 'function') {
            throw new Error('Expected the enhancer to be a function.');
          }
          return enhancer(createStore)(reducer, initialState);
        }
        if (typeof reducer !== 'function') {
          throw new Error('Expected the reducer to be a function.');
        }
```
```js
        var currentReducer   = reducer;
        var currentState     = initialState;
        var currentListeners = [];
        var nextListeners    = currentListeners;
        var isDispatching    = false;
```
这些都是闭包变量
```js
        function ensureCanMutateNextListeners() {
          if (nextListeners === currentListeners) {
            nextListeners = currentListeners.slice();
          }
        }

        function getState() {
          return currentState;
        }
```
返回当前的state
```js
        function subscribe(listener) {
          if (typeof listener !== 'function') {
            throw new Error('Expected listener to be a function.');
          }
          var isSubscribed = true;
          ensureCanMutateNextListeners();
          nextListeners.push(listener);
          return function unsubscribe() {
            if (!isSubscribed) {
              return;
            }
            isSubscribed = false;
            ensureCanMutateNextListeners();
            var index = nextListeners.indexOf(listener);
            nextListeners.splice(index, 1);
          };
        }      
```
注册listener，同时返回一个取消事件注册的方法
```js
        function dispatch(action) {
          if (!(0, _isPlainObject2["default"])(action)) {
            throw new Error('Actions must be plain objects. ' + 'Use custom middleware for async actions.');
          }
          if (typeof action.type === 'undefined') {
            throw new Error('Actions may not have an undefined "type" property. ' + 'Have you misspelled a constant?');
          }
          if (isDispatching) {
            throw new Error('Reducers may not dispatch actions.');
          }
          try {
            isDispatching = true;
            currentState  = currentReducer(currentState, action);
          } finally {
            isDispatching = false;
          }
          var listeners = currentListeners = nextListeners;
          for (var i = 0; i < listeners.length; i++) {
            listeners[i]();
          }
          return action;
        }
```
通过action该改变state，然后执行subscribe注册的方法
```js
        function replaceReducer(nextReducer) {
          if (typeof nextReducer !== 'function') {
            throw new Error('Expected the nextReducer to be a function.');
          }
          currentReducer = nextReducer;
          dispatch({type: ActionTypes.INIT});
        }
```
替换reducer，修改state变化的逻辑
```js
        function observable() {
          var _ref;
          var outerSubscribe = subscribe;
          return _ref = {
            subscribe: function subscribe(observer) {
              if (typeof observer !== 'object') {
                throw new TypeError('Expected the observer to be an object.');
              }

              function observeState() {
                if (observer.next) {
                  observer.next(getState());
                }
              }

              observeState();
              var unsubscribe = outerSubscribe(observeState);
              return {unsubscribe: unsubscribe};
            }
          }, _ref[_symbolObservable2["default"]] = function () {
            return this;
          }, _ref;
        }

        dispatch({type: ActionTypes.INIT});
```
初始化时，执行内部一个dispatch，得到初始state
```js
        return _ref2 = {
          dispatch      : dispatch,
          subscribe     : subscribe,
          getState      : getState,
          replaceReducer: replaceReducer
        }, _ref2[_symbolObservable2["default"]] = observable, _ref2;
      }
    },
    function (module, exports) {
      'use strict';
      exports.__esModule = true;
      exports["default"] = warning;
      function warning(message) {
        if (typeof console !== 'undefined' && typeof console.error === 'function') {
          console.error(message);
        }
        try {
          throw new Error(message);
        } catch (e) {
        }
      }
    },
    function (module, exports, __webpack_require__) {
      var getPrototype     = __webpack_require__(8),
          isHostObject     = __webpack_require__(9),
          isObjectLike     = __webpack_require__(10);
      var objectTag        = '[object Object]';
      var objectProto      = Object.prototype;
      var funcToString     = Function.prototype.toString;
      var hasOwnProperty   = objectProto.hasOwnProperty;
      var objectCtorString = funcToString.call(Object);
      var objectToString   = objectProto.toString;

      function isPlainObject(value) {
        if (!isObjectLike(value) ||
          objectToString.call(value) != objectTag || isHostObject(value)) {
          return false;
        }
        var proto = getPrototype(value);
        if (proto === null) {
          return true;
        }
        var Ctor = hasOwnProperty.call(proto, 'constructor') && proto.constructor;
        return (typeof Ctor == 'function' &&
        Ctor instanceof Ctor && funcToString.call(Ctor) == objectCtorString);
      }

      module.exports = isPlainObject;
    },
    function (module, exports, __webpack_require__) {
      'use strict';
      exports.__esModule = true;
      var _extends       = Object.assign || function (target) {
          for (var i = 1; i < arguments.length; i++) {
            var source = arguments[i];
            for (var key in source) {
              if (Object.prototype.hasOwnProperty.call(source, key)) {
                target[key] = source[key];
              }
            }
          }
          return target;
        };
      exports["default"] = applyMiddleware;
      var _compose       = __webpack_require__(1);
      var _compose2      = _interopRequireDefault(_compose);

      function _interopRequireDefault(obj) {
        return obj && obj.__esModule ? obj : {"default": obj};
      }

      function applyMiddleware() {
        for (var _len = arguments.length, middlewares = Array(_len), _key = 0; _key < _len; _key++) {
          middlewares[_key] = arguments[_key];
        }
        return function (createStore) {
          return function (reducer, initialState, enhancer) {
            var store         = createStore(reducer, initialState, enhancer);
            var _dispatch     = store.dispatch;
            var chain         = [];
            var middlewareAPI = {
              getState: store.getState,
              dispatch: function dispatch(action) {
                return _dispatch(action);
              }
            };
            chain             = middlewares.map(function (middleware) {
              return middleware(middlewareAPI);
            });
            _dispatch         = _compose2["default"].apply(undefined, chain)(store.dispatch);

            return _extends({}, store, {
              dispatch: _dispatch
            });
          };
        };
      }
    },
    function (module, exports) {
      'use strict';
      exports.__esModule = true;
      exports["default"] = bindActionCreators;
      function bindActionCreator(actionCreator, dispatch) {
        return function () {
          return dispatch(actionCreator.apply(undefined, arguments));
        };
      }

      function bindActionCreators(actionCreators, dispatch) {
        if (typeof actionCreators === 'function') {
          return bindActionCreator(actionCreators, dispatch);
        }
        if (typeof actionCreators !== 'object' || actionCreators === null) {
          throw new Error('bindActionCreators expected an object or a function, instead received ' + (actionCreators === null ? 'null' : typeof actionCreators) + '. ' + 'Did you write "import ActionCreators from" instead of "import * as ActionCreators from"?');
        }
        var keys                = Object.keys(actionCreators);
        var boundActionCreators = {};
        for (var i = 0; i < keys.length; i++) {
          var key           = keys[i];
          var actionCreator = actionCreators[key];
          if (typeof actionCreator === 'function') {
            boundActionCreators[key] = bindActionCreator(actionCreator, dispatch);
          }
        }
        return boundActionCreators;
      }
    },
    function (module, exports, __webpack_require__) {
      'use strict';
      exports.__esModule  = true;
      exports["default"]  = combineReducers;
      var _createStore    = __webpack_require__(2);
      var _isPlainObject  = __webpack_require__(4);
      var _isPlainObject2 = _interopRequireDefault(_isPlainObject);
      var _warning        = __webpack_require__(3);
      var _warning2       = _interopRequireDefault(_warning);

      function _interopRequireDefault(obj) {
        return obj && obj.__esModule ? obj : {"default": obj};
      }

      function getUndefinedStateErrorMessage(key, action) {
        var actionType = action && action.type;
        var actionName = actionType && '"' + actionType.toString() + '"' || 'an action';
        return 'Given action ' + actionName + ', reducer "' + key + '" returned undefined. ' + 'To ignore an action, you must explicitly return the previous state.';
      }

      function getUnexpectedStateShapeWarningMessage(inputState, reducers, action) {
        var reducerKeys  = Object.keys(reducers);
        var argumentName = action && action.type === _createStore.ActionTypes.INIT ? 'initialState argument passed to createStore' : 'previous state received by the reducer';
        if (reducerKeys.length === 0) {
          return 'Store does not have a valid reducer. Make sure the argument passed ' + 'to combineReducers is an object whose values are reducers.';
        }
        if (!(0, _isPlainObject2["default"])(inputState)) {
          return 'The ' + argumentName + ' has unexpected type of "' + {}.toString.call(inputState).match(/\s([a-z|A-Z]+)/)[1] + '". Expected argument to be an object with the following ' + ('keys: "' + reducerKeys.join('", "') + '"');
        }
        var unexpectedKeys = Object.keys(inputState).filter(function (key) {
          return !reducers.hasOwnProperty(key);
        });
        if (unexpectedKeys.length > 0) {
          return 'Unexpected ' + (unexpectedKeys.length > 1 ? 'keys' : 'key') + ' ' + ('"' + unexpectedKeys.join('", "') + '" found in ' + argumentName + '. ') + 'Expected to find one of the known reducer keys instead: ' + ('"' + reducerKeys.join('", "') + '". Unexpected keys will be ignored.');
        }
      }

      function assertReducerSanity(reducers) {
        Object.keys(reducers).forEach(function (key) {
          var reducer      = reducers[key];
          var initialState = reducer(undefined, {type: _createStore.ActionTypes.INIT});
          if (typeof initialState === 'undefined') {
            throw new Error('Reducer "' + key + '" returned undefined during initialization. ' + 'If the state passed to the reducer is undefined, you must ' + 'explicitly return the initial state. The initial state may ' + 'not be undefined.');
          }
          var type = '@@redux/PROBE_UNKNOWN_ACTION_' + Math.random().toString(36).substring(7).split('').join('.');
          if (typeof reducer(undefined, {type: type}) === 'undefined') {
            throw new Error('Reducer "' + key + '" returned undefined when probed with a random type. ' + ('Don\'t try to handle ' + _createStore.ActionTypes.INIT + ' or other actions in "redux/*" ') + 'namespace. They are considered private. Instead, you must return the ' + 'current state for any unknown actions, unless it is undefined, ' + 'in which case you must return the initial state, regardless of the ' + 'action type. The initial state may not be undefined.');
          }
        });
      }

      function combineReducers(reducers) {
        var reducerKeys   = Object.keys(reducers);
        var finalReducers = {};
```
提出不合法的reducers, finalReducers就是一个闭包变量
```js
        for (var i = 0; i < reducerKeys.length; i++) {
          var key = reducerKeys[i];
          if (typeof reducers[key] === 'function') {
            finalReducers[key] = reducers[key];
          }
        }
        var finalReducerKeys = Object.keys(finalReducers);
        var sanityError;
        try {
          assertReducerSanity(finalReducers);
        } catch (e) {
          sanityError = e;
        }
        return function combination() {
          var state  = arguments.length <= 0 || arguments[0] === undefined ? {} : arguments[0];
          var action = arguments[1];
          if (sanityError) {
            throw sanityError;
          }
          if (true) {
            var warningMessage = getUnexpectedStateShapeWarningMessage(state, finalReducers, action);
            if (warningMessage) {
              (0, _warning2["default"])(warningMessage);
            }
          }
          var hasChanged = false;
          var nextState  = {};
          for (var i = 0; i < finalReducerKeys.length; i++) {
            var key                 = finalReducerKeys[i];
            var reducer             = finalReducers[key];
            var previousStateForKey = state[key];
            var nextStateForKey     = reducer(previousStateForKey, action);
            if (typeof nextStateForKey === 'undefined') {
              var errorMessage = getUndefinedStateErrorMessage(key, action);
              throw new Error(errorMessage);
            }
            nextState[key] = nextStateForKey;
            hasChanged     = hasChanged || nextStateForKey !== previousStateForKey;
          }
          return hasChanged ? nextState : state;
        };
```
`combination`函数，一个总reducer，内部包含子reducer
```js
      }
    },
    function (module, exports) {
      var nativeGetPrototype = Object.getPrototypeOf;

      function getPrototype(value) {
        return nativeGetPrototype(Object(value));
      }

      module.exports = getPrototype;
    },
    function (module, exports) {
      function isHostObject(value) {
        var result = false;
        if (value != null && typeof value.toString != 'function') {
          try {
            result = !!(value + '');
          } catch (e) {
          }
        }
        return result;
      }

      module.exports = isHostObject;
    },
    function (module, exports) {
      function isObjectLike(value) {
        return !!value && typeof value == 'object';
      }

      module.exports = isObjectLike;
    },
    function (module, exports, __webpack_require__) {
      (function (global) {
        'use strict';
        module.exports = __webpack_require__(12)(global || window || this);
      }.call(exports, (function () {
        return this;
      }())))
    },
    function (module, exports) {
      'use strict';
      module.exports = function symbolObservablePonyfill(root) {
        var result;
        var Symbol = root.Symbol;
        if (typeof Symbol === 'function') {
          if (Symbol.observable) {
            result = Symbol.observable;
          } else {
            if (typeof Symbol['for'] === 'function') {
              result = Symbol['for']('observable');
            } else {
              result = Symbol('observable');
            }
            Symbol.observable = result;
          }
        } else {
          result = '@@observable';
        }
        return result;
      };
    }
  ])
});
;
```
