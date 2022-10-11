# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar)

此项目是学习B站up 不止前端的杨y 的羊了个羊小游戏开发
一开始时是打算做成微信小程序，但是由于元素定位设置出错 + 个别函数过于冗杂，已经不好再进行删减、优化，所以就将技术选型换成了Vue3，在开始的时候都很顺利，只是到了三连消除算法的时候，由于对Vue3掌握程度不够，导致面对被淘汰的filter过滤时，不知道用什么进行替代，查阅资料后，大部分工程师都选择了用计算属性进行过滤，但是由于我自身能力不足，换了多种使用computed的方式都没能完好解决，但也对组合式API的使用有了更深的了解
虽然最终没能用Vue3进行开发，但在写的过程中收获了很多心得（幼稚的学习笔记），如果您有兴趣的话，可以往下看看！

  一、解析parseInt(Math.random() * (max - min + 1) + min);
	
      在原生JS中，
      parseInt(Math.random()*10); 的意思是可均衡获取0到9的随机整数。

      Math.floor(Math.random()*(max-min)+min);
      与
      parseInt(Math.random()*(max-min)+min);的意思是生成[min,max]
      的随机数
      故而parseInt(Math.random() * (max - min + 1) + min)的意思是
      生成[min, max+1]的随机整数
      
      
      二、
      1、如果想在js中获取图片路径并在template中动态调用，就必须先用import引入图片资源文件或者
      使用require（这个方法vue3好像不能直接使用）


      2、两次for循环嵌套能够使得图片以m x m的格式出现


      3、<div class="block" @click="Select(item, index, $event)" v-for="(item, index) in       allblock" :key="index" :style="{left: item.left, top: item.top}">
      中Select()的参数由于v-for的存在而有具体值，就是v-for得到的item与index值，$event是
      传输本身的事件，$event打印出来后会有一长串结果，而关于 图片块点击事件 的具体内容会保存在一       个名为srcElement的返回值中，而在后台操作关于图片块的时候，我们优先选择使用item参数，毕竟
      这是图片块的原有全部信息，而非 图片块点击事件 的信息


      4、设置选择图片块的数组selectblock = []，该数组主要用于保存选中的图片块的信息，


      5、在Select方法中，我们需要再次获取到 allblock数组(该数组内保存着显示部分的全部图片快)，       利用splice()方法，将选中的图片删除
          具体做法是： Select(item, index, event) {
                allblock.splice(index, i)
                }
        这就是很经典的splice用法，我想这不需要过多笔记，值得注意的点就是此处的index由               allblock[]中得到，点击方法又通过点击事件获得，再传输给allblock[]用于删除，非常巧妙。


      6、紧接着我们可以进行下一步，即将选中的图片块显示到下方的选择栏中。还记得上面我们设置的          selectblock = []吗，这里我们可以在allblock删除前先将选中的图片块数据push进该数组
      再在html中显示出来
          具体做法是：Select(item, index, event) {
                selectblock.push(item);			
                }
      在这两步中，我们的数组并没有用到this.作为指向，希望你能记住，这是因为我们正在使用vue3


      7、此时遇到了一个大问题，虽然大，但是很愚蠢，我希望你以后能牢牢记住
        “不要忘了你使用的是vue3!!!”
         由于我们前面定义数据时忘记利用reactive定义响应式数据，所以在数据成功改变后视图渲染并没      有发生更新，正确定义数组应该是：
          let allblock =reactive([])  //全部图片块
          let selectblock = reactive([]) //选中的图片块
