title: Js模板
author: Asia Cui
tags:
  - Js
categories:
  - Js模板
date: 2019-01-18 09:53:00
---
## 第一种模板

```javascript
var demo = {
	/**
	* 数据域
	**/
    url: {
        host:"localhost",
        port: "80",
        uri: "/demo/user/get"
    },
	/**
	* 初始化操作
	**/
    init: function () {
       //调用function
	   demo.handle(demo.url.host);
    },
	/**
	* 业务处理
	**/
	handle :function(data){
		//do...
		return data;
	}
};
jQuery(document).ready(function () {
    //方法入口
    demo.init();
});

```

## 第二种模板

### 实体类

```javascript
/**
 * 部门实体类
 */
var Dept = {
    id: "",	//表格id
    name: null
};

```

### 函数体

```javascript
/**
 * 初始化
 */
Dept.init = function () {
    //do...
    return null;
};
/**
 * 删除操作
 */
Dept.delete = function () {
    //do...
};

/**
 * 函数调用
 */
$(function () {
    Dept.init();
    Dept.delete();
});

```

