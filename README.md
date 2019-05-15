---

---

## 基础服务

包括高阶组件授权，表单条件搜索显示栏，面包屑显示栏，表单操作按钮显示栏

#### 技术栈

------

react + antd + react-router-dom + webpack + ES6/7 + less + svg 

#### 项目部署

------

 ![http://192.168.17.162:4873/](https://simpleicons.org/icons/npm.svg)

私库访问地址:[http://192.168.17.162:4873](http://192.168.17.162:4873)

```npm install winning-basic-service 
npm install winning-basic-service 
or 
yarn add winning-basic-service(推荐) 

import {Global} from winning-basic-service
```



#### 关于接口数据

------

此项目的所有接口数据都来源于外部引入

<a name="说明" ></a>

#### 使用文档

------



#### API

#####  `import {Global} from '#/MegrezII'`

全局工具方法类

-  `Global.alert`

给出页面提示，可以选择是否成功提示，可以调用回调函数

```
     /**
     * @param result 接口统一的返回格式
     * @param successFun 提示成功之后，关闭窗口时回调的接口
     * @param isSuccessAlert 是否提示成功页面
     */
     Global.alert(result, {successFun, isSuccessAlert = true} = {})
```

-  `Global.alert`

操作 增/删数组，返回最新数组

```
     /**
     * @param oldArr 旧数组
     * @param newArr 新数组
     * @param isAdd 是否合并 true:合并 false:移除
     * @returns {*} 返回合并或
     */
     Global.operateArr: ({oldArr = [], newArr = []}, isAdd = true)
```

-  `Global.`



​		

​		

​		

​		





