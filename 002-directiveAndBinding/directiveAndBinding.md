##代码部分请下载本文代码阅读，代码均能正常运行并有详细的注释。
[本文代码下载地址](https://github.com/hekui-github/learnVUE/tree/master/001-Jquery-VS-Vue)

##概述
无需置疑，Vue之所以能如此之火，主要受益于它是一个MVVM框架和它易学的文档，几乎所有觉得学习Vue有难度的开发者都是觉得组件脚手架等等不太好理解，但是这些都不是Vue的核心，Vue的核心表现在最易学的Vue指令和绑定，学好Vue指令和绑定基本就学好Vue的一大半了，把基础核心的几个知识点说完会做几个完整的demo。

#####学习方法
   学习编程切勿眼高手低，碰到简单的如Vue指令类似的知识千万不要忽略掉，最好能把指令熟记于心，对于新手更是这样，这样在讲解复杂知识点的时候就不用再重述这些简单的知识点了，当你对于简单知识点烂熟于心的时候，你的注意力会集中在少数几个最难的知识点上，更容易学好，所以读者最好能对着下面的代码和官方文档把这些指令多理解多记忆，再学习后面的文章。

#####[Vue指令的官方文档](https://cn.vuejs.org/v2/api/#指令)

#####Vue指令大杂烩
```
    <style>
      [cloak] {
        display: none;
      }

    </style>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
    <div id="demo">
        <!-- 插值就是把{{}}中的值当做一个变量，然后再用vue实例data中对应的变量值替换它 -->
        插值用法: {{demo1}} <hr>
        <!-- v-text指令 的功能和{{}}的功能一模一样，只是为了方便把这个指令单独拿出来用{{}}表示 -->
        v-text用法: <span v-text = "demo2"></span> <hr>
        <!-- v-html 类式 v-text， 只是把这个变量字符串取出来渲染成了对应的html，而不是直接展示字符串 -->
        v-html用法: <span v-html = "demo3"></span> <hr>
        <!-- v-show对应变量的值应该是true或false,如果为true则自动跟他添加display：block，否则添加display：none ,你只需要修改对应变量的值就可以控制元素显示隐藏-->
        v-show用法：<span v-show = "demo4">这是v-show内容，v-show的值控制这个元素显示隐藏</span> <hr>
        <!-- v-if和v-show的用法类式，v-if的值s是true直接添加到dom元素，否则从dom商remove掉，而不是修改display属性，这样操作比较消耗性能，如无必要请使用v-show -->
        v-if用法：  <span v-if = "demo5">这是v-if的内容</span><hr>
        <!-- v-else 必须跟v-if联合使用，就像js中的else也必须跟if一起使用一样，如果v-if元素的内容显示，则v-else元素remove掉，反之v-if元素remove掉，v-else元素添加到dom上 -->
        v-else用法： <span v-if = "demo6">这是v-if的内容</span> <span v-else >这是v-else的内容</span>  <hr>
        <!-- v-else-if 必须跟v-if联合使用，如果v-if的值为和v-else-if的值都为false则显示v-else-if元素的内容,请参考js中的if、else if、else理解 -->
        v-else-if用法：<span v-if = "demo71">这是v-if的内容</span> <span v-else-if = "demo72" >这是v-else-if的内容</span> <span v-else> 这是v-else内容</span><hr>
        <!-- v-for 和js中的for in循环很类似，有了v-for你可以轻松复制多个当前元素，有以下几种写法，前面两种是for循环数组的，后两种是循环json的，都有待index项和不带的 -->
        <!-- <div v-for = "item in items">  -->
        <!-- <div v-for="(item, index) in items"> -->
        <!-- </div> <div v-for="(val, key) in object"></div> -->
        <!--  <div v-for="(val, key, index) in object"></div> -->
        v-for用法1： <span v-for="(item, index) in items1">{{"这是第"+index + "个" + ":" + item}}</span> <br>
        v-for用法2： <span v-for="(value,key,index) in items2" >{{"key:" +key + "|" +"value:" + value + "|" + "index:" + index + "     " }}</span> <hr>
        <!-- v-on 是绑定事件的方法，v-on:click="clickMethod"是c点击事件，它可以简写成@click ,它的值是methods中的方法 -->
        v-on用法：  <input type="text" name="name" v-on:keyup= "keyupEvent" style="border:solid 1px red;" /><hr>
        <!-- v-bind 后面接的是html标签所带的属性，比如v-bind:value 可以省略为:value ，v-bind是单向数据绑定，意思是你改变了vue实例data中的数据，页面dom中的值就会变化，反之不成立  -->
        v-bind用法：<input v-bind:value="demo8" style="border:solid 1px red;" /> <input :value="demo8" style="border:solid 1px red;" /><span>改变input的值并不能修改vue实例中的值，所以另一个input不会修改</span><hr>
        <!-- v-model 和 v-bind 类似，v-model是双向数据绑定， 但是当dom改变的时候vue实例data中的数据也会改变，所以叫双向数据绑定 -->
        v-model用法： <input v-model:value="demo9" style="border:solid 1px red;" /> <input v-model:value="demo9" style="border:solid 1px red;" /><span>改变input的值能修改vue实例中的值，所以另一个input会修改,这叫双向数据绑定</span><hr>
        <!-- 和html中的pre标签类似,{{item}}这种插值就会当做字符串处理，并不会解析成变量 -->
        v-pre用法：<span v-pre> {{demo10}}</span><hr>
        <!-- 由于vue编译渲染需要时间，在vue编译完之前，{{item}}还是以字符串的形式表现出来的，v-cloak 配合[v-cloak]{display:none} 可以让元素在vue编译前先隐藏 -->
        v-cloak用法：<span v-cloak>{{demo11}}</span> | <span>{{demo11}}</span><hr>
        <!-- v-once 指令表示此元素只渲染一次，当data中的数据值改变的时候这个元素不会再改变渲染 -->
        v-once用法：
        <span v-once>{{demo12}}</span>

    </div>
    <script>
      //new 一个vue实例，Vue就能运行了,Vue方法传入参数为一个约定格式的json
      new Vue({
        //el表示这个vue实例的作用范围为id为demo的元素内
        el:"#demo",
        //data的值是一个json，存储的都是要显示到页面上的值，可以理解成这里放的都是js变量(数据)
        data:{
          //插值用法
          demo1:"这是demo1",
          //v-text用法：
          demo2:"这是demo2",
          //v-html用法
          demo3:"<span style='color:red'>这是v-html字符串，它是一个span</span>",
          //v-show用法：
          demo4:true,
          //v-if用法：
          demo5:true,
          //v-else用法：
          demo6:false,
          //v-else-if用法：
          demo71:false,
          demo72:true,
          //v-for用法：
          items1:["item1","itme2","item3"],
          items2:{key1:"value1",key2:"value2",kay3:"value3"},
          //v-bind用法：
          demo8:"这是demo8",
          //v-model用法：
          demo9:"这是demo9",
          //v-pre用法：
          demo10:"这是demo10",
          //v-cloak用法：
          demo11:"这是demo11"
          //v-once用法：
          demo12:"这是deom12"
        },
        methods:{
          //v-on用法：
          keyupEvent:function() {
            console.log("keyup被促发了");
          }
        },
       
      });
    </script>

```