---
title: "图书管理系统总结"
date: 2018-10-03T00:46:23+08:00
---

### 简介

&emsp;&emsp;前端和后端分别由 [Vue.js](https://cn.vuejs.org/) 和 php 编写，由 [axios](https://www.npmjs.com/package/axios) 传递数据，界面样式使用了 [Purecss](https://purecss.io/) 。

<!--more-->

![home](https://s1.ax1x.com/2018/10/03/i3KlJe.png "home")

***
<!-- TOC -->

- [简介](#简介)
- [前端](#前端)
    - [菜单栏](#菜单栏)
    - [表格](#表格)
    - [下拉框](#下拉框)
        - [文本框的动态绑定](#文本框的动态绑定)
    - [搜索框](#搜索框)
- [后端](#后端)

<!-- /TOC -->
***
### 前端
&emsp;&emsp;页面主体由菜单栏和主要内容构成，都是使用 Vue + axios 动态获取的。

#### 菜单栏
    <div v-html="menu" id="menu"></div>

    <script>
      axios.get("./menu.html")
        .then(function (response) {
          var menu = new Vue({
            el: '#menu',
              data: {
                'menu': ''
              }
          });
          menu.menu = response.data;
        });
    </script>
&emsp;&emsp;这里要注意的是，由 Vue 渲染出来的 [v-html](https://cn.vuejs.org/v2/api/#v-html) 的内容中 Vue 模板 **不会再次被 Vue 编译** ，也就是说 `menu.html` 中的 Vue 代码不会有效。

#### 表格
    <table>
      <thead>
        ...
      </thead>
      <tbody id="table">
        <tr v-for="item in items">
          <td v-for="part in item">{{ part }}</td>
        </tr>
      </tbody>
    </table>

    <script>
      var table = new Vue({
        el: '#table',
        data: {
          'items': []
        }
      })
      axios.post("./search.php", {
        ...
        })
        .then(function (response) {
          table.items = response.data;
        });
    </script>
&emsp;&emsp;表格的处理使用了嵌套的 [v-for](https://cn.vuejs.org/v2/api/#v-for) 。

#### 下拉框
    <div id="lend">
      <input list="mbl" v-model="member">
      <datalist id="mbl">
        <option v-for="item in items" v-bind:value="item.id">
          {{ item.name + ',' + item.phone }}
        </option>
      </datalist>
    </div>

    <script>
      var lend = new Vue({
        el: '#lend',
        data: {
          'items': []
        }
      })
      axios.post("./search.php?index=1", {
        ... 
        })
        .then(function (response) {
          lend.items = response.data;
        });
    </script>
&emsp;&emsp;这里面请求参数 `index=1` 要求保留后端 php 传回关联数组的键值。

##### 文本框的动态绑定
    <div id="edit">
      <input list="mbl" v-model="id" v-on:change="update">
      <datalist id="mbl">
        ...
      </datalist>
      <input v-model="name">
      <input v-model="phone">
    </div>

    <script>
      var edit = new Vue({
        el: '#edit',
        data: {
          'id': '',
          'name': '',
          'phone': ''
        },
        methods: {
          update: function () {
            this.name = this.items[this.id]['name'];
            this.phone = this.items[this.id]['phone'];
          }
        }
      })
    </script>
&emsp;&emsp;之前要求过保留关联数组的键值，所以可以用 `this.id` 直接作为每一项的索引。

#### 搜索框
    <div id="search">
      <select v-model="type">
        <option value="id">...</option>
        <option value="name">...</option>
        <option value="title">...</option>
        <option value="date">...</option>
        <option value="time">...</option>
        <option value="return">...</option>
        <option value="rdate">...</option>
      </select>
      <div v-if="type == 'date' || type == 'rdate'">
        ...
      </div>
      <div v-else-if="type == 'time' || type == 'id'">
        ...
      </div>
      <select ... v-else-if="type == 'return'">
        ...
      </select>
      <input  ... v-else>
      <select ... v-if="type != 'return'">
        ...
      </select>
    </div>

    <script>
      var search = new Vue({
        el: '#search',
        data: {
          'type': 'id',
          ...
        }
      })
    </script>
&emsp;&emsp;根据选择的关键字类型改变搜索框的形式，使用 [v-if](https://cn.vuejs.org/v2/api/#v-if) [v-else-if](https://cn.vuejs.org/v2/api/#v-else-if) [v-else](https://cn.vuejs.org/v2/api/#v-else) 根据 type 选择显示的样式。

### 后端
&emsp;&emsp;主要的问题就是前端 axios post 的数据是 json 格式，不是 x-www-form-urlencoded 格式，不能用 `$_POST` 来接收。

    $data = json_decode(file_get_contents('php://input'), true);

这里用 `$data` 来代替 `$_POST`。