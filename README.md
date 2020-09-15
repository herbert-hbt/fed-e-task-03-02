# 简答题

## 请简述 Vue 首次渲染的过程。

答：过程

* 初始化Vue, 给Vue挂载一系列原型和静态方法
* 创建根实例new Vue(options)
* 构造函数中调用_init方法
    - 挂载各种实例方法
    - 触发一系列生命周期钩子
    - 调用$mount挂载dom
        - 处理el
        - 模板编译（如果有需要）
        - 创建渲染watcher
        - 触发updateComponent
            - 调用vm._render生成虚拟dom -> createElement
            - 调用vm.patch挂载dom

## 请简述 Vue 响应式原理。

答：

* 入口在_init方法的initState的initData中
* 通过调用observe方法，将数据转为响应式数据
    - 通过new Dep创建观察对象
    - 定义getter，将观察者存储到观察对象，观察者为渲染watcher对象
    - 定义setter，进行派发更新，通知所有观察者调用update方法
* 在$mount中，创建渲染watcher时，
    - 通过获取属性值，触发getter中的依赖收集，
    - 通过将当前watcher赋值给Dep的静态属性target，指定依赖为当前watcher
* 当属性被赋值时，触发setter，循环调用当前观察对象的所有观察者（watcher）的update方法，在update方法中进行dom更新

## 请简述虚拟 DOM 中 Key 的作用和好处。

答：

1. 减少dom操作
2. 减少diff和渲染所需要的时间
3. 提升性能

## 请简述 Vue 中模板编译的过程。

答：

* 合并模板编译选项
* 将模板字符串转为ast
* 遍历ast，优化ast，标识静态节点和静态根节点
* 遍历ast，拼接生成js代码的字符串形式
* 通过调用new Function(), 将字符串形式代码转为js代码
