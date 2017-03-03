[本文代码下载地址](https://github.com/hekui-github/learnVUE/tree/master/001-Jquery-VS-Vue)

##概述
#####前端开发近况
- 需求依然旺盛，从JavaScript已经在编程语言排行榜上排到了第七位和前端聘岗位数就可以看出。
- 加入前端开发的新手越来越多，其中女孩子比例不少，自学能力稍有匮乏
- 前端框架层出不穷，部分前端开发精力跟不上
- 作为一名码农最急需的是精通一门语言一个框架，然后再横向去尽量多学一些技术，有助于融会贯通，专业精通才有高收入。

#####写这个博客的目的
- 希望能通过写博客分享的方式更好的学习Vue和其它前端技术
- 希望能帮助到更多的同学更快速的学习Vue和其它前端技术
- 希望能赚点零花钱，如果你觉得我的文章帮助到了你，请在文章最下面点打赏按钮打赏我。打赏过的同学加我qq:791831347,我拉你进我建的Vue交流群算是小小的回报吧，你在群里问的问题都会尽量得到解答，但不做任何承诺。
- 未来也可能计划出一套视频教程
- 让我们一起走在Vue进阶的路上吧.(这个博客我会尽量说人话少说专业术语)

##Vue简述
长期以来，前端都是Jquery为王这样一个状态，虽然从业时间比较长的前沿的前端开发者可能都已经接触至少十多个框架了，但是大多数年轻的开发者可能都还只是对Jquery这样的万金油更熟悉一些，下面我会用几个小例子来说明Jquery开发和Vue这样的Mvvm框架开发模式上的不同。

#####用一个简单的例子来说明编写Jquery和Vue上的不同
######demo1 简单修改文字
- 点击按钮后：
把hello，美女！欢迎学习Angular.
 改为
 hello，帅哥！欢迎学习Vue.
 
![屏幕快照 2017-02-27 22.48.32.png](http://upload-images.jianshu.io/upload_images/1024196-b43cbfbcd31198c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- jquery代码(用以下代码直接替换掉html文件中的body,看不懂没关系后面会详细说道Vue的方方面面)

``` 
<div>
    <p>Hello, <span id="name">美女</span>!</p>
    <p>欢迎学习 <span id="libName">Angular</span>.</p>
    <button id = "modifyBtn">修改</button>
</div>

<script src="https://unpkg.com/jquery"></script>
<script type="text/javascript">
	$("#modifyBtn").click(function(){
		$("#name").text("帅哥");
		$("#libName").text("Vue");
	});
</script>
```

- Vue代码


```
<div id="mycontent">
	<p>Hello, <span >{{name}}</span>!</p>
	<p>欢迎学习 <span >{{libName}}</span>.</p>
	<button v-on:click="modifyInfo" >修改</button>
</div>

<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script type="text/javascript">
	var vm = new Vue({
		el: "#mycontent",
		data:{
			name:"美女",
			libName:"Angular"
		},
		methods:{
			modifyInfo:function(){
				this.name = "帅哥";
				this.libName = "Vue";
			}
		}
	})
</script>
```

- tips 1  Jquery首先要获取到dom对象，然后对dom对象进行进行值的修改等操作；
- tips 2. Vue是首先把值和js对象进行绑定，然后修改js对象的值，Vue框架就会自动把dom的值就行更新。
- tips 3. 可以简单的理解为Vue帮我们做了dom操作，我们以后用Vue就需要修改对象的值和做好元素和对象的绑定，Vue这个框架就会自动帮我们做好dom的相关操作。
- tips 4.这种dom元素跟随JS对象值的变化而变化叫做单向数据绑定，如果JS对象的值也跟随着dom元素的值的变化而变化就叫做双向数据绑定，顾名思义单向和双向😊，后面会详细介绍。

#####用一个更复杂的例子来说明Vue的优势
######demo2-模拟一个简易购物车

![QQ20170303-144554@2x.png](http://upload-images.jianshu.io/upload_images/1024196-79299dedbd583511.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
下面只是展示下两种框架写出来的代码，感兴趣的同学可以把代码下下来。
-Vue代码
```
<div id="cart_item">
	<!-- v-for 可以根据Vue中myListArr数组的长度来生成div,其中item和index分别是每一项的值和索引 -->
	 <div class="item_i" v-for="(item,index) in myListArr">
	    <div class="checkbox"  >
	    	<!-- v-on:click绑定了check点击方法,传入了index这个参数，它是数组当前的索引，在外层div的v-for中定义过 -->
	        <input type="checkbox" class="checkOne check" v-model="item.isChecked"  v-on:click="check(index)"/></div>
	    <div class="good">
	    	<!-- 绑定了图片的地址和商品的名称,:src 是v-bind:src的简写,只要是html元素的属性值都可以用v-bind指令 -->
	        <img :src ="item.pic"  />
	        <span>{{item.name}}</span></div>
	    <div class="price">{{item.price}}</div>
	    <div class="count">
	    	<!-- @click 是v-on:click的简写,这里特意展示一下 -->
	        <span class="reduce" @click="reduce(index)">-</span>
	        <input type="text" class="count_input" v-model="item.num" />
	        <span class="add" v-on:click="add(index)">+</span></div>
	    <!-- 这里每一行商品总价是价格*数量，四舍五入保留两位小数 -->
	    <div class="subTotal">{{(item.price* item.num).toFixed(2)}}</div>
	    <div class="act">
	        <span class="delete" @click= "remove(index)">删除</span></div>
	</div>
</div>

var vm = new Vue({
	el: "#cart",
	data:{
		 myListArr : [
							{
								name:"【三只松鼠_小贱拉面丸子85gx3】休闲零食膨化小吃干脆面串烧味",
								pic:"https://gw.alicdn.com/bao/uploaded/i1/TB16tk_KpXXXXX7XVXXXXXXXXXX_!!0-item_pic.jpg_200x200q50s150.jpg_.webp",
								price:22.8,
								num:2,
								isChecked:true,

							},
							{
								name:"【三只松鼠_炭烧腰果185gx2袋】坚果零食特产炒货干果腰果仁",
								pic:"https://gw.alicdn.com/bao/uploaded/i2/TB1372RKXXXXXXnXVXXXXXXXXXX_!!0-item_pic.jpg_200x200q50s150.jpg_.webp",
								price:42,
								num:1,
								isChecked:true,
							},
							{
								name:"【三只松鼠_皇族牌牛奶夹心饼240g】台湾进口休闲零食夹心饼干",
								pic:"https://gw.alicdn.com/bao/uploaded/i3/TB1UK3uOVXXXXaiaXXXXXXXXXXX_!!0-item_pic.jpg_200x200q50s150.jpg_.webp",
								price:15.5,
								num:3,
								isChecked:true,

							},
							{
								name:"【三只松鼠_碧根果210gx2袋】零食坚果山核桃长寿果干果奶油味",
								pic:"https://gw.alicdn.com/bao/uploaded/i2/TB1eTzgJVXXXXcIXFXXXXXXXXXX_!!0-item_pic.jpg_200x200q50s150.jpg_.webp",
								price:18.9,
								num:1,
								isChecked:true,

							}],
		 //这三个属性分别绑定的是所有商品数量、总价格、时候全选
		 totalCount:0,
		 totalPrice:0,
		 allCheck:true,
	},
	mounted: function () {
        //这里是vue初始化完成后执行的函数,注意vue1.x版本是ready方法,如无特别说明本人使用的都是Vue2.x
        this.calTotal();
    },
	methods:{
		//每一行增加商品的方法,根据索引值修改这一项对应的数据源的值就可以了，Vue会帮你自动更新dom中相关的值
		add:function(index){
			var item = this.myListArr[index];
			item.num +=1;
			//计算总价和总件数
			this.calTotal();
		},
		//减商品
		reduce:function(index){
			var item = this.myListArr[index];
			//如果商品只有1件就不允许再减了，只能删除
			if (item.num == 1) {
				return;
			}
			item.num -= 1;
			this.calTotal();
		},
		//删除本行商品
		remove:function(index) {
			//splice 是array的批量删除方法
			this.myListArr.splice(index,1);
			this.calTotal();
		},
		//单行的checkbox选中
		check:function(index){
			var listItem = this.myListArr[index];
			this.calTotal();
			if (!listItem.isChecked) {
				//如果没有选中状态肯定是没有全选
				this.allCheck = false;
			}else {
				//如果是选中状态先把全选选中，然后再每一项遍历，如果有一项没有选中就改为非全选状态
				this.allCheck = true;

				for (var i = 0; i < this.myListArr.length; i++) {
					var listItem1 = this.myListArr[i];
					if (!listItem1.isChecked) {
						this.allCheck = false;
					}
				}
			}
		},
		//全选checkbox事件
		allCheckMethod:function(){
			//全选只需要所有的列表都跟全选状态是同一个状态就可以
			for (var i = 0; i < this.myListArr.length; i++) {
				var listItem = this.myListArr[i];
				listItem.isChecked = this.allCheck;
			}
			this.calTotal();
		},
		//计算总数
		calTotal:function(){
			//计算总件数
			this.calTotalCount();
			//计算总价格
			this.calTotalPrice();
		},
		//计算总件数
		calTotalCount: function () {
	    	var count = 0;
	    	for (var i = 0; i < this.myListArr.length; i++) {
	    		var listItem = this.myListArr[i];
	    		if (listItem.isChecked) {
	    			count += listItem.num;
	    		}
	    	}
	      	this.totalCount = count;
	    },
	    //计算总价格
	    calTotalPrice: function () {
	    	var price = 0.0;
	    	for (var i = 0; i < this.myListArr.length; i++) {
	    		var listItem = this.myListArr[i];
	    		if (listItem.isChecked) {
	    			 price = price + listItem.num * listItem.price;
	    		}
	    	}
	    	this.totalPrice = price;
	    }
	},
});
```
- Jquery代码
```
$(function(){
		//这里模拟数据，实际中是ajax请求网络数据，并没有太大差异
		var myListArr = [{
							name:"【三只松鼠_小贱拉面丸子85gx3】休闲零食膨化小吃干脆面串烧味",
							pic:"https://gw.alicdn.com/bao/uploaded/i1/TB16tk_KpXXXXX7XVXXXXXXXXXX_!!0-item_pic.jpg_200x200q50s150.jpg_.webp",
							price:22.8,
							num:2,},
						{
							name:"【三只松鼠_炭烧腰果185gx2袋】坚果零食特产炒货干果腰果仁",
							pic:"https://gw.alicdn.com/bao/uploaded/i2/TB1372RKXXXXXXnXVXXXXXXXXXX_!!0-item_pic.jpg_200x200q50s150.jpg_.webp",
							price:42,
							num:1,},
						{
							name:"【三只松鼠_皇族牌牛奶夹心饼240g】台湾进口休闲零食夹心饼干",
							pic:"https://gw.alicdn.com/bao/uploaded/i3/TB1UK3uOVXXXXaiaXXXXXXXXXXX_!!0-item_pic.jpg_200x200q50s150.jpg_.webp",
							price:15.5,
							num:3,},
						{
							name:"【三只松鼠_碧根果210gx2袋】零食坚果山核桃长寿果干果奶油味",
							pic:"https://gw.alicdn.com/bao/uploaded/i2/TB1eTzgJVXXXXcIXFXXXXXXXXXX_!!0-item_pic.jpg_200x200q50s150.jpg_.webp",
							price:18.9,
							num:1,}];
		//每个列表项对应的html代码，实际情况中只要先把html写好然后拷贝去空格就好
		var listItemCodeStr = '<div class="item_i"><div class="checkbox"><input type="checkbox" class="checkOne check" checked="checked"></div><div class="good">![]( 1.jpg)<span>xxxxxxx</span></div><div class="price">0</div><div class="count"><span class="reduce">-</span><input type="text" class="count_input" value="1"><span class="add">+</span></div><div class="subTotal">0</div><div class="act"><span class="delete">删除</span></div></div>';

		//根据数据来添加每一项列表到dom上
		for (var i = 0; i < myListArr.length; i++) {
			//生成的列表项dom元素
			var jqueryListItem = $(listItemCodeStr);
			//对应列表项数据
			var listItemData = myListArr[i];

			//用数据填充dom列表项
			fillListWithData(jqueryListItem,listItemData)

			//把列表项添加到dom上
			$("#cart_item").append(jqueryListItem);
		}

		//跟两个全选check-box加事件
		$('.checkAll').click(function(event) {
			var checkList = $(".checkOne");
			var checkAllList = $(".checkAll");

			//让另一个按钮也全选或者也不全选保持同步
			for (var i = 0; i < checkAllList.length; i++) {
				checkAllList.get(i).checked = this.checked;
			}

			for (var i = 0; i < checkList.length; i++) {
				//如果当前项和全选项不一样则执行选中单行操作
				if (checkList.get(i).checked != this.checked) {
					checkList.get(i).click();
				}
			}
		});

	});
```
```
//返回每一行的数据
	function getTotalCountAndPrice (item) {
		var count_input = parseInt(item.find(".count_input").eq(0).val());
		var price = parseFloat(parseFloat(item.find(".price").eq(0).text()).toFixed(2));
		var totalPrice = parseFloat((count_input*price).toFixed(2));

		var jsonData = {"count":count_input,"price":price,"totalPrice":totalPrice};
		return jsonData;
	}
//每一个商品的总价
	function getSubTotal(item){
		var listData = getTotalCountAndPrice(item);		
		item.find('.subTotal').eq(0).html(listData.totalPrice);
	}
//根据每一行的数据传入修改所有商品总价格和总件数
	function updateTotal(item,count){
		var listData = getTotalCountAndPrice(item);		
		var listPrice = listData.price;

		var totalPrice = parseFloat($("#totalPrice").eq(0).text());
		var totalCount = parseInt($("#totalCount").eq(0).text());

		$("#totalPrice").text((totalPrice + count * listPrice ).toFixed(2));
		$("#totalCount").text((totalCount + count ));
	}
```
```
//根据列表项数据填充列表项和总价总数量
	function fillListWithData(jqueryListItem,listItemData){
		//首次跟一行列表赋值(一个商品)
		jqueryListItem.find('img').eq(0).attr('src', listItemData.pic);
		jqueryListItem.find('span').eq(0).html(listItemData.name);
		jqueryListItem.find('.price').eq(0).html(listItemData.price);
		jqueryListItem.find('.count_input').eq(0).val(listItemData.num);
		//计算一行的总价格
		getSubTotal(jqueryListItem);
		//减商品个数的事件
		jqueryListItem.find('.reduce').click(function(event) {
			//个数输入框，因为会取值赋值用到几次，所以提出来作变量
			var count_inputOBJ =  $(this).parent().find('.count_input').eq(0);
			var count_input = parseInt(count_inputOBJ.val());
			//输入框的值为1就不允许再减个数了，输入框最小值为1
			if (count_input == 1) {
				return;
			}
			count_input -= 1;
			count_inputOBJ.val(count_input);
			//更新每一行的总价格
			getSubTotal($(this).parent().parent());
			var itemCheck = $(this).parent().parent().find(".checkOne").get(0);
			if (itemCheck.checked) {
				//如果这个商品勾选了应该更新整个总价格和总数量
				updateTotal($(this).parent().parent(),-1);
			}
		});
		//增加商品个数的事件,代码同减商品个数不注释
		jqueryListItem.find('.add').click(function(event) {
			var count_inputOBJ =  $(this).parent().parent().find('.count_input').eq(0);
			var count_input = parseInt(count_inputOBJ.val());
			count_input += 1;
			count_inputOBJ.val(count_input);
			getSubTotal($(this).parent().parent());
			var itemCheck = $(this).parent().parent().find(".checkOne").get(0);
			if (itemCheck.checked) {
				updateTotal($(this).parent().parent(),1);
			}
		});
		//删除某个商品的事件,代码同加减商品个数不注释
		jqueryListItem.find('.delete').click(function(event) {

			var itemCheck = $(this).parent().parent().find(".checkOne").get(0);
			if (itemCheck.checked) {
				var count_inputOBJ =  $(this).parent().parent().find('.count_input').eq(0);
				updateTotal($(this).parent().parent(),- parseInt(count_inputOBJ.val()));
			}
			$(this).parent().parent().remove();
		});
		//跟列表项的check-box加事件
		jqueryListItem.find('.checkOne').click(function(event) {
			var listData = getTotalCountAndPrice($(this).parent().parent());
			if (this.checked) {
				//加上勾选项数量和价格
				updateTotal($(this).parent().parent(),listData.count);
				//遍历看是否是全选
				var checkList = $(".checkOne");
				var checkAllList = $(".checkAll");

				var allCheckTag = true ;
				for (var i = 0; i < checkList.length; i++) {
					var checkItem = checkList.get(i);
					if (!checkItem.checked) {
						allCheckTag = false;
						break;
					}
				}
				if (allCheckTag) {
					for (var j = 0; j < checkAllList.length; j++) {
						checkAllList.get(j).checked = true;
					}
				}
			}else {
				//减去勾选项
				updateTotal($(this).parent().parent(),-listData.count);
				//去掉全选
				var checkAllList = $(".checkAll");
				for (var j = 0; j < checkAllList.length; j++) {
					checkAllList.get(j).checked = false;
				}

			}
		});
		//初始化总价,每循环一个列表项就应该把总数总价格更新下
		 updateTotal(jqueryListItem,listItemData.num);
	}
```
#####总结
如果你有认真写一下以下代码，可能你会在再做类似的项目的时候再也不想使用Jquery,下面做一下对比：
1.由于Vue帮我们省略了dom操作，代码变得比较简洁，逻辑更加清晰。
2.还是由于Vue帮我们省略了dom操作，加上双向数据绑定，Vue的代码量减少很多，大概2/3(还是要看具体项目)。
3.Jquery 专注于dom操作，步骤一般为：获取dom元素--> 跟dom元素赋值+加事件-->插入dom元素。 其中dom元素赋值和加事件又需要获取dom元素和更dom元素赋值，也就是这个原因代码量才会增加。Vue专注于数据：用户只需要关注dom元素值对应绑定的数据，每次dom需要修改只需要去修改数据就可以了。
4.由于多个dom事件可能会同时修改一个元素的值，Vue只需要关注元素对应绑定的数据就可以了，这使得Vue在逻辑上会更加清晰

#####读完这篇文章我希望您已经学会了：
1.知道Vue是以数据为中心的，你只需要关注数据，比类Jquery的优势在于去dom操作和双线数据绑定。
2.知道Vue.js的基本写法，例如怎么引入vue.js，怎么声明Vue实例，实例中能挂载的参数有el、methods，data等，el、methods、data又分别表示什么，methods内部的方法this可以引用Vue实例等等
3.了解到基本的Vue指令，比如v-model、v-on:click、@click、v-for、v-bind:src、:src,还有{{}}和@click方法里面可以传参等等
4.最后希望你能把这个demo好好敲一敲，不管你理不理解代码，熟练是学好一个框架的第一步，看再多听再多，不如好好写一下，有问题留言。



如果您觉得这篇文章对您有帮助，请打赏一下或去github上给个❤️，thanks。