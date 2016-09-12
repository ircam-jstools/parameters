# ParametersJS

> Tiny and extendable library to type check class / object parameters.

## Install

```
npm install [--save] parameters
```

## Usage

```js
const definitions = {
  myBooleanParam: {
    type: 'boolean',
    default: false,
    constant: false,
    metas: { kind: 'static' }
  },
  myIntegerParam: {
    type: 'integer',
    min: 0,
    max: Infinity,
    default: 0,
    constant: false,
    metas: {
      kind: 'static',
      shortDescr: 'My First Integer Param',
      fullDescr: 'This parameter is my first integer parameter in this example.',
      unit: '%',
      step: 1,
      max: 100,
    }
  },
  // ...
};

class MyClass {
  constructor(options) {
    this.params = parameters(definitions, options);

    this.params.addListener((name, value, metas) => {
      // ...
    });

    this.params.addParamListener('myIntegerParam', (value, metas) => {
      // ...
    });
  }
}

const myInstance = new MyClass({ myIntegerParam: 42 });

const bValue = myInstance.params.get('myBooleanParam');
> false

const iValue = myInstance.params.get('myIntegerParam');
> 42

myInstance.params.set('myIntegerParam', definitions.myIntegerParam.min - 1);
```

# API

<a name="ParameterBag"></a>

## ParameterBag
Bag of parameters. Main interface of the library

**Kind**: global class  

* [ParameterBag](#ParameterBag)
    * _instance_
        * [.getDefinitions()](#ParameterBag+getDefinitions) ⇒ <code>Object</code>
        * [.get(name)](#ParameterBag+get)
        * [.set(name, value)](#ParameterBag+set)
        * [.reset([name])](#ParameterBag+reset)
        * [.addListener(callback)](#ParameterBag+addListener)
        * [.removeListener(callback)](#ParameterBag+removeListener)
        * [.addParamListener(name, callback)](#ParameterBag+addParamListener)
        * [.removeParamListener(name, callback)](#ParameterBag+removeParamListener)
    * _inner_
        * [~listenerCallback](#ParameterBag..listenerCallback) : <code>function</code>
        * [~paramListenerCallack](#ParameterBag..paramListenerCallack) : <code>function</code>

<a name="ParameterBag+getDefinitions"></a>

### parameterBag.getDefinitions() ⇒ <code>Object</code>
Return the given definitions along with the initialization values.

**Kind**: instance method of <code>[ParameterBag](#ParameterBag)</code>  
<a name="ParameterBag+get"></a>

### parameterBag.get(name)
Return the value of the given parameter.

**Kind**: instance method of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>String</code> | Name of the parameter. |

<a name="ParameterBag+set"></a>

### parameterBag.set(name, value)
Set the value of a parameter. If the value of the parameter is updated
(aka if previous value is different from new value) all registered
callbacks are registered.

**Kind**: instance method of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>String</code> | Name of the parameter. |
| value | <code>Mixed</code> | Value of the parameter. |

<a name="ParameterBag+reset"></a>

### parameterBag.reset([name])
Reset a parameter to it's init value. Reset all if name is `null`

**Kind**: instance method of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [name] | <code>String</code> | <code></code> | Name of the parameter to reset. |

<a name="ParameterBag+addListener"></a>

### parameterBag.addListener(callback)
Add listener to all param updates.

**Kind**: instance method of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Description |
| --- | --- | --- |
| callback | <code>ParameterBag~listenerCallack</code> | Listener to register. |

<a name="ParameterBag+removeListener"></a>

### parameterBag.removeListener(callback)
Remove listener from all param changes.

**Kind**: instance method of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| callback | <code>ParameterBag~listenerCallack</code> | <code></code> | Listener to remove. If  `null` remove all listeners. |

<a name="ParameterBag+addParamListener"></a>

### parameterBag.addParamListener(name, callback)
Add listener to a given param updates.

**Kind**: instance method of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>String</code> | Parameter name. |
| callback | <code>[paramListenerCallack](#ParameterBag..paramListenerCallack)</code> | Function to apply  when the value of the parameter changes. |

<a name="ParameterBag+removeParamListener"></a>

### parameterBag.removeParamListener(name, callback)
Remove listener from a given param updates.

**Kind**: instance method of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| name | <code>String</code> |  | Parameter name. |
| callback | <code>[paramListenerCallack](#ParameterBag..paramListenerCallack)</code> | <code></code> | Listener to remove.  If `null` remove all listeners. |

<a name="ParameterBag..listenerCallback"></a>

### ParameterBag~listenerCallback : <code>function</code>
**Kind**: inner typedef of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Description |
| --- | --- | --- |
| name | <code>String</code> | Parameter name. |
| value | <code>Mixed</code> | Updated value of the parameter. |
| [meta=] | <code>Object</code> | Given meta data of the parameter. |

<a name="ParameterBag..paramListenerCallack"></a>

### ParameterBag~paramListenerCallack : <code>function</code>
**Kind**: inner typedef of <code>[ParameterBag](#ParameterBag)</code>  

| Param | Type | Description |
| --- | --- | --- |
| value | <code>Mixed</code> | Updated value of the parameter. |
| [meta=] | <code>Object</code> | Given meta data of the parameter. |

<a name="parameters"></a>

## parameters : <code>function</code>
Factory for the `ParameterBag` class

**Kind**: global typedef  

| Param | Type | Description |
| --- | --- | --- |
| definitions | <code>Object.&lt;String, paramDefinition&gt;</code> | Object describing the  parameters. |
| values | <code>Object.&lt;String, Mixed&gt;</code> | Initialization values for the  parameters. |

<a name="parameters.defineType"></a>

### parameters.defineType(typeName, parameterDefinition)
Register a new type for the `parameters` factory.

**Kind**: static method of <code>[parameters](#parameters)</code>  

| Param | Type | Description |
| --- | --- | --- |
| typeName | <code>String</code> | Value that will be available as the `type` of a  param definition. |
| parameterDefinition | <code>parameterDefinition</code> | Object describing the  parameter. |

<a name="booleanDefinition"></a>

## booleanDefinition : <code>Object</code>
**Kind**: global typedef  
**Properties**

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| type | <code>String</code> | <code>&#x27;boolean&#x27;</code> | Define a boolean parameter. |
| default | <code>Boolean</code> |  | Default value of the parameter. |
| constant | <code>Boolean</code> | <code>false</code> | Define if the parameter is constant. |
| metas | <code>Object</code> | <code>{}</code> | Optionnal metadata of the parameter. |



## License

BSD-3-Clause
