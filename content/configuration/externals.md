---
title: Externals
sort: 13
contributors:
  - sokra
  - skipjack
  - pksjce
  - dear-lizhihua
---

`externals` configuration in webpack provides a way of not including a dependency in the bundle. Instead the created bundle relies on that dependency to be present in the consumers environment.
This typically applies to library developers though application developers can make good use of this feature too.

## `externals`

`string` `regex` `function` `array` `object`

**防止打包**某些 `import` 的包(package)，而是*在运行时再去获取这些外部扩展包(package)*。

例如，从 CDN 引入 [jQuery](https://jquery.com/)，而不是把它打包在源文件中：

**index.html**

```html
...
<script src="https://code.jquery.com/jquery-3.1.0.js"
  integrity="sha256-slogkvB1K3VOkzAI8QITxV3VzpOnkeNVsKvtkYLMjfk="
  crossorigin="anonymous"></script>
...
```

**webpack.config.js**

```javascript
externals: {
  jquery: 'jQuery'
}
```

这样就脱离了那些不需要改动的独立模块，换句话，下面展示的代码还可以正常运行：

```javascript
import $ from 'jquery';

$('.my-element').animate(...);
```

T> __consumer__ here is any end user application that includes the library that you have bundled using webpack.

Your bundle which has external dependencies can be used in various module contexts mainly [CommonJS, AMD, global and ES2015 modules](/concepts/modules). The external library may be available in any of the above form but under different variables.

`externals` supports the following module contexts

  * __global__ - An external library can be available as a global variable. The consumer can achieve this by including the external library in a script tag. This is the default setting for externals.
  * __commonjs__ -  The consumer application may be using a CommonJS module system and hence the external library should be available as a CommonJS module.
  * __amd__ - Similar to the above line but using AMD module system.

`externals` accepts various syntax and interprets them in different manners.

### string

`jQuery` in the externals indicates that your bundle will need `jQuery` variable in the global form.

### array

```javascript
externals: {
  subtract: ['./math', 'subtract']
}
```

`subtract: ['./math', 'subtract']` converts to a parent child construct, where `./math` is the parent module and your bundle only requires the subset under `subtract` variable.

### object

```javascript
externals : {
  react: 'react'
}

// or

externals : {
  lodash : {
    commonjs: "lodash",
    amd: "lodash",
    root: "_" // indicates global variable
  }
}
```

This syntax is used to describe all the possible ways that an external library can be available. `lodash` here is available as `lodash` under AMD and CommonJS module systems but available as `_` in a global variable form.

### function

?> TODO - Not sure how to use it in and function form. Would be great to see a sample.

### regex

?> TODO - I think its overkill to list externals as regex.

For more information on how to use this configuration, please refer to the article on [how to author a library](/guides/author-libraries).

***

> 原文：https://webpack.js.org/configuration/externals/