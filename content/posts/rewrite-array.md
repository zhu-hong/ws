---
title: "重写Array方法"
date: 2021-11-03T14:42:46+08:00
draft: true
tags: [rewrite]
categories: [JavaScript]
code:
  maxShownLines: 30
---

## 深拷贝
```javascript
function deepClone(origin, target) {
  var tar = target || {};
  var toStr = Object.prototype.toString;
  var arrType = '[object Array]';

  for (var k in origin) {
    if (origin.hasOwnProperty(k)) {
      if (typeof origin[k] === 'object' && origin[k] !== null) {
        tar[k] = toStr.call(origin[k]) === arrType ? [] : {};

        deepClone(origin[k], tar[k]);
      } else {
        tar[k] = origin[k];
      }
    }
  }

  return tar;
}
```

## forEach

```javascript
Array.prototype.forEachRw = function (cb) {
  var _arr = this;
  var _len = _arr.length;
  var _this = arguments[1] || window;

  for (var i = 0; i < _len; i++) {
    cb.apply(_this, [_arr[i], i, _arr]);
  }
}
```

## map

```javascript
Array.prototype.mapRw = function (cb) {
  var _arr = this;
  var _len = _arr.length;
  var _this = arguments[1] || window;
  var _newArr = [];

  for (var i = 0; i < _len; i++) {
    var _item = typeof _arr[i] === 'object' ? deepClone(_arr[i]) : _arr[i];
    var _res = cb.apply(_this, [_item, i, _arr]);
    _res && _newArr.push(_res);
  }

  return _newArr;
}
```

## filter

```javascript
Array.prototype.filterRw = function (cb) {
  var _arr = this;
  var _len = _arr.length;
  var _this = arguments[1] || window;
  var _newArr = [];

  for (var i = 0; i < _len; i++) {
    var _item = typeof _arr[i] === 'object' ? deepClone(_arr[i]) : _arr[i];
    cb.apply(_this, [_item, i, _arr]) && _newArr.push(_item);
  }

  return _newArr;
}
```

## every
```javascript
Array.prototype.everyRw = function (cb) {
  var _arr = this;
  var _len = _arr.length;
  var _this = arguments[1] || window;
  var _res = true;

  for (var i = 0; i < _len; i++) {
    if (!cb.apply(_this, [_arr[i], i, _arr])) {
      _res = false;
      break;
    }
  }

  return _res;
}
```

## some
```javascript
Array.prototype.someRw = function (cb) {
  var _arr = this;
  var _len = _arr.length;
  var _this = arguments[1] || window;
  var _res = false;

  for (var i = 0; i < _len; i++) {
    if (cb.apply(_this, [_arr[i], i, _arr])) {
      _res = true;
      break;
    }
  }

  return _res;
}
```

## reduce
```javascript
Array.prototype.reduceRw = function (cb, initialValue) {
  var _arr = this;
  var _len = _arr.length;
  var _this = arguments[2] || window;

  for (var i = 0; i < _len; i++) {
    var _item = typeof _arr[i] === 'object' ? deepClone(_arr[i]) : _arr[i];
    initialValue = cb.apply(_this, [initialValue, _item, i, _arr]);
  }

  return initialValue;
}
```