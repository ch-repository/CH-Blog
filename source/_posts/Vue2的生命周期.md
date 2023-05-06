---
title: Vue2的生命周期
date: 2023-01-29 17:25:22
tags:
    - JavaScript
    - Vue
categories: Vue
comments: false
---

Vue2生命周期整理

Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂在DOM -> 渲染、更新 -> 渲染、卸载等一系列过程，这个过程就是Vue的生命周期。

1. beforeCreate(创建前)：数据观测和初始化事件还未开始，此时data的响应式追踪、event/watcher都还没有被设置，也就是说不能访问到data、computed、watch、methods上的方法和数据。
2. create(创建后)：实例创建完成，实例上配置的options包括data、computed、watch、methods等都配置完成，但是此时渲染的节点还未挂在到DOM，所以不能范文到$el属性。
3. beforeMount(挂载前)：在挂载开始之前被调用，相关的render函数首次被调用。实例已完成以下的配置：编辑模板，把data里面的数据和模板生成html。此时还没有挂载html到页面上。
4. mounted(挂载后)：在el被新创建的vm.$el替换，并挂载到实例上去之后调用。实例已完成一下的配置：用上面编译好的html内容跟替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互。
5. beforeUpdate(更新前)：响应式数据更新时调用，此时虽然响应式数据更新了，但是对应的真实DOM还没有被渲染。
6. updated(更新后)：在犹豫数据更改导致的虚拟DOM重新渲染和打补丁之后调用。此时DOM已经根据响应式数据的变化更新了。调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无线循环。该钩子在服务器渲染期间不被调用。
7. beforeDestroy(销毁前)：实例销毁之前调用。这一步，实例仍然完全可用，this仍能获取到实例。
8. destroyed(销毁后)：实例销毁后调用，调用后，Vue实例指示的所有东西都会被解绑，所有的事件鉴定其会被移除，所有的子实例也会被销毁。该钩子在服务器渲染期间不被调用。

领完还有keep-alive组件独有的生命周期，分别为activated和deactivated。用keep-alive包裹的组件在切换时不会被进行销毁，而是缓存到内存中并执行deactivated钩子函数，命中缓存渲染后会执行activated钩子函数。
