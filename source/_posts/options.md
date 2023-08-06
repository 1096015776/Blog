---
title: think options
date: 2023-08-06 15:33:54
tags:
  - javascript
  - tip
---

## why

1. 在初始化class实例的时候，我希望用户传入需要参数去更改，其余参数而使用一套默认配置,不必传入完整的配置表

### eg:

  ```javascript
  // 假设我们有一个class为Person
  // 初始化时
  let defaultOptions = {
    name:'fx',
    travel: {
        city: 'AnShan',
        next:null
    }
  }

  //配置重写
  let overWriteOptions = {
    name: 'jack',
    travel: {
        desc:'去鞍山转转'
    }
  }

  // how to get wantedOptions
  let wantedOptions = {
    name:'jack',
    travel: {
        city: 'AnShan',
        desc:'去鞍山转转',
        next:null
    }
  }

  ```

## how

1. Object.assign 处理赋值逻辑?
    在简单的对象中，似乎不存在问题，可如果存在引用对象，会完全覆盖，而并不是我们想要的功能

  ```javascript
  let rs = Object.assign(defaultOptions,overWriteOptions)
  console.log(rs)
  //{ name: 'jack', travel: { desc: '去鞍山转转' } }
  //problem travel 属性为引用类型 赋值会导致全部覆盖
  ```

2.需要一个类似assign的函数去处理引用类型
  ```javascript
    import pkg from 'lodash'
    const { isEmpty, isObject } = pkg
    let assignObject = (options, overWriteOptions) => {
        if (isEmpty(options)) {
            return overWriteOptions
        } else if (isEmpty(overWriteOptions)) {
            return options
        } else {
            for (let key in overWriteOptions) {
                if (isEmpty(options[key]) || !isObject(options[key])) {
                    options[key] = overWriteOptions[key]
                } else {
                    assignObject(options[key], overWriteOptions[key])
                }
            }
            return options
        }
    }
    let rs = assignObject(defaultOptions,overWriteOptions)
    console.log(rs)
    //{ name: 'jack', travel: { city: 'AnShan', next: null, desc: '去鞍山转转' } }
  ```

  3.这个问题似乎解决了，但是数据结构中涉及到链表中的,如果是循环的，也就是对象当中存在了环，这似乎就被卡死了。因此这里还需要再做一次处理,希望在环形结构的对象中也是直接赋值

### eg:
  ```javascript
    let travel1 = {
        city: 'AnShan',
        next: null,
    }
    let travel2 = {
        city: 'NanJing',
        next: null,
    }
    travel1.next = travel2
    travel2.next = travel1
    let defaultOptions = {
        name: 'fx',
        travel: {
            city: 'AnShan',
            next: travel1,
        },
    }
    let overWriteOptions = {
        name: 'jack',
        travel: travel2,
    }
    let rs = assignObject(defaultOptions, overWriteOptions)
    console.log(rs) 
    //Maximum call stack size exceeded
  ```

  4.我需要一个函数检查引用对象是否存在环，环形结构为直接赋值(应该涉及到map的遍历),目前还没思考周全，等研究明白再补充


