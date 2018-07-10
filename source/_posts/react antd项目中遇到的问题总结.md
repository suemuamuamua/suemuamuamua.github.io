---
title: react项目中遇到的问题汇总
date: 2018-07-10 14:02:18
tags:
      --react
categories: JavaScript
---
此次系统bug修改中遇到很多问题，现将部分问题总结于此，方便以后学习。
<!-- more -->
1. 父组件调用子组件 antd form的方法
由于子组件的form经过 Form.create ，之后如果父组件要拿到 ref，可以使用 rc-form 提供的 wrappedComponentRef。
~~~js
// use wrappedComponentRef
<childForm wrappedComponentRef={(form) => this.form = form} />
this.form.props.form.validateFileds({force: true}) // =>调用子组件的验证表单
~~~

2. modal中的表单元素select等的定位问题
antd中表单元素的下拉框默认定位容器为body，想要滚动表单时需将该空间的定位容器设置为Modal。时间选择器同理。
~~~js
<Select getPopupContainer={()=>document.querySelector('.ant-modal')} />
<DatePicker getCalendarContainer={()=>document.querySelector('.ant-modal')} />
~~~