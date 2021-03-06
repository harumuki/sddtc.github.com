---
title: "[React] 当我在写 React 我在写什么。"
layout: post
categories: sddtc tech
guid: urn:uuid:988e1743-8b55-404e-acdd-520ab7e971c2
tags:
  - javascript
  - react
---

### 背景

如果你有这样的经历，要在一周左右的时间对一个功能小而美的代码库添加一个完整的功能模块。  
而你没有相关的技术栈经验，该怎么样去整理保证最快的实践曲线呢？我有了一些小小的感触。  

### 心路历程
#### 感触一
我对于 `React` 的理解，仅仅停留在是一个流行且强大的东西。  
那么在工作里，快速实现一个符合产品的需求是一件不会很难的事情，难的是**正确的使用**一个你不熟悉的东西。

#### 弯路一
什么时候用 `state`，什么时候用 `props` ？

#### 直球一
`React` 是一个JS库，库意味着较好的封装性、使用性，可说到底，也是基于JS这门语言，万变不离其宗才是真谛。  

**什么时候用 `state` ?**   
什么时候用我还没有实践，但是尽量不用是我的一个体会。因为我在使用 `window` 时把它放在了 `state` 里， 现在想想很可笑，而当时是因为不理解 `state`，并不是可以实现功能就是正确的实践。   

**Tips**: `state` 是为了维护组件内部的数据状态，用于组件保存、控制以及修改自己的状态，修改 `state` 属性会导致组件的重新渲染。 [Where to Initialize State in React](https://daveceddia.com/where-initialize-state-react/)

*没有state的叫做无状态组件，有state的叫做有状态组件*  

**什么时候用 `props` ?**   
`props` 是组件一个非常方便的特性，从外部传入参数写入组件内部读取、处理。组件的 `props` 尽量保持精简，组件之间的属性保持简洁、解耦。而我犯的一个错误是过多的给组件添加了 `props` 属性，换一句话说，用不到的 `props` 一定不要传递给组件。

**Tips**: `props` 经常被用作渲染组件和初始化状态，当一个组件被实例化之后，它的props是只读的，不可改变的。如果props在渲染过程中可以被改变，会导致这个组件显示的形态变得不可预测。只有通过父组件重新渲染的方式才可以把新的props传入组件中。

#### 感触二  
测试必要且有用。  

#### 弯路二  
在你对这项技术栈理解不深的时候，测试框架会再一次逼迫你的时间并增加你的学习成本，那么，逃避不好吗？ 于是我犯的第二个错就是逃避了测试。当然，是在一个"合理"的理由之下。

#### 直球二
编写测试，就像你在实现真实的业务逻辑，它能帮助你精简你不擅长的技术栈的逻辑实现，循环迭代自己的代码。  
后来我完成了相关的测试，并体会到了其中的美妙。🙂

#### 感触三  
尽快的实现不是不好，而是会在后期暴露一些问题，在实现之前，稳重的学习。

### 总结
通过两周的时间，走了很多弯路，有些感受，也有些思考，组件和组件之间可以怎样合理的交互？  
肯定很多人会觉得，有那么多 quickStart，明明是可以做到更好的。  
那么时间怎么悄悄流逝，是我发现的一个关于自身更重要的一个问题。啊，人生啊。😔  



