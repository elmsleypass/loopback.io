---
title: "Model hooks"
lang: en
layout: page
keywords: LoopBack
tags:
sidebar: lb2_sidebar
permalink: /doc/en/lb2/Model-hooks.html
summary:
---

{% include warning.html content="

**Model hooks are deprecated, except for [afterInitialize](/doc/en/lb2/Model-hooks.html).**

**Please use [operation hooks](/doc/en/lb2/Operation-hooks.html)** **instead**.

" %}

**See also**:

* [Remote methods](/doc/en/lb2/Remote-methods.html)
* [Remote hooks](/doc/en/lb2/Remote-hooks.html)
* [Operation hooks](/doc/en/lb2/Operation-hooks.html)
* [Connector hooks](/doc/en/lb2/Connector-hooks.html)
* [Tutorial: Adding application logic](/doc/en/lb2/Tutorial-Adding-application-logic.html) 

## Overview

Use model hooks to add custom logic to models that extend [PersistedModel](http://apidocs.strongloop.com/loopback/#persistedmodel).
Each hook is called before or after a specific event in the model's lifecycle.

You can define the following model hooks, listed in the order that the events occur in a model lifecycle:

* `afterInitialize` - triggers after a model has been initialized.
* `beforeValidate` - triggers before validation is performed on a model. 
* `afterValidate` - triggers after validation is performed on a model.
* `beforeSave` - triggers before a model is saved to a data source.
* `afterSave` - triggers after a model is saved to a data source.
* `beforeCreate` - triggers before a model is created.
* `afterCreate` - triggers after a model is created.
* `beforeUpdate` - triggers before a model is updated.
* `afterUpdate` - triggers after a model is updated.
* `beforeDestroy` - triggers before a model is destroyed.
* `afterDestroy` - triggers after a model is destroyed.

Best practice is to register model hooks in `/common/models/your-model.js`. This ensures hooks are registered during application initialization.
If you need to register a hook at runtime, get a reference to the `app` object and register it right then and there.

## afterInitialize

This hook is called after a model is initialized.

{% include important.html content="

This model hook is _not_ deprecated and is still useful. It is a synchronous method: there is no callback function.

" %}

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.afterInitialize = function() {
  //your logic goes here
};
//...
```

Most operations require initializing a model before actually performing an action, but there are a few cases where the initialize event is not triggered,
such as HTTP requests to the `exists`, `count`, or bulk update REST endpoints.

{% include important.html content="

This is the only hook that does not require you to explicitly call `next()` after performing your logic.

" %}

## beforeValidate

This hook is called before [validatation](/doc/en/lb2/Validating-model-data.html) is performed on a model.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.beforeValidate = function(next, modelInstance) {
  //your logic goes here - don't use modelInstance
  next();
};
//...
```

{% include important.html content="

In the beforeValidate hook, use `this` instead of `modelInstance` to get a reference to the model being validated. In this hook, `modelInstance` is not valid.

" %}

You must call `next()` to let LoopBack now you're ready to go on after the hook's logic has completed.

{% include warning.html content="

If you don't call `next()`, the application will appear to \"hang\".

" %}

## afterValidate

This hook is called after [validation](/doc/en/lb2/Validating-model-data.html) is performed on a model.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.afterValidate(next) {
  //your logic goes here
  next();
};
//...
```

You must call `next()` to let LoopBack now you're ready to go on after the hook's logic has completed.

## beforeCreate

This hook is called just before a model is created.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.beforeCreate = function(next, modelInstance) {
  //your logic goes here
  next();
};
//...
```

LoopBack provides `modelInstance` as a reference to the model being created.  

You must call `next()` to continue execution after the hook completes its logic. If you don't the application will appear to hang.

## afterCreate

This hook is called after a model is created.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.afterCreate = function(next) {
  //your logic goes here
  this.name = 'New coffee shop name; //you can access the created model via `this`
  next();
};
//...
```

Access the model being created with `this`.

You must call `next()` to continue execution after the hook completes its logic. If you don't the application will appear to hang.

## beforeSave

This hook is called just before a model instance is saved.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.beforeSave = function(next, modelInstance) {
  //your logic goes here
  next();
};
//...
```

LoopBack provides `modelInstance` as a reference to the model being saved.

You must call `next()` to continue execution after the hook completes its logic. If you don't the application will appear to hang.

## afterSave

This hook is called after a model is saved.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.afterSave = function(next) {
  //your logic goes here
  this.name = 'New coffee shop name; //you can access the created model via `this`
  next();
};
//...
```

Access the model being saved with `this`.

You must call `next()` to continue execution after the hook completes its logic. If you don't the application will appear to hang.

## beforeUpdate

This hook is called just before a model is updated.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.beforeUpdate = function(next, modelInstance) {
  //your logic goes here
  next();
};
//...
```

LoopBack provides `modelInstance` as a reference to the model being updated.

You must call `next()` to continue execution after the hook completes its logic. If you don't the application will appear to hang.

## afterUpdate

This hook is called after a model is updated.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.afterUpdate = function(next) {
  //your logic goes here
  this.name = 'New coffee shop name; //you can access the created model via `this`
  next();
};
//...
```

LoopBack provides `modelInstance` as a reference to the model being saved.

You must call `next()` to continue execution after the hook completes its logic. If you don't the application will appear to hang.

## beforeDestroy

This hook is called just before a model is destroyed.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.beforeDestroy = function(next, modelInstance) {
  //your logic goes here
  next();
};
//...
```

LoopBack provides `modelInstance` as a reference to the model being saved.

You must call `next()` to continue execution after the hook completes its logic. If you don't the application will appear to hang.

## afterDestroy

This hook is called after a model is destroyed.

### Example

**/common/models/coffee-shop.js**

```javascript
//...
CoffeeShop.afterDestroy = function(next) {
  //your logic goes here
  next();
};
//...
```

You must call `next()` to continue execution after the hook completes its logic. If you don't the application will appear to hang.