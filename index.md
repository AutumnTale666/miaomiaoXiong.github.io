 _Welcome to AutumnTale666'sweb blog_
 # 微信小程序之--（与唯品会来场粉红色的邂逅  🎉🎉🎉）
 买买买，虽然双十二刚过，可是唯品会的折扣却是依然火爆。一打开页面，便是粉色的主页映入眼帘，琳琅满目的商品，让我这个月光族过了把眼瘾。虽然买不起，那就自己模仿着写一个，把喜欢的商品一个个推进小推车里。（😍）下面为大家分享一个把唯品会里面的精致商品推进自己购物车的微信小程序，🙈
 
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/luping.gif)
 
## 在上面的录屏演示中，主要是实现购物车功能，话不多说，扔个代码看看：
 
### 主页面: 轮播图片这个小技巧比较普遍的被使用，代码如下：
 
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/img/4.jpg)
 
 #### index.wxml:使用swiper组件,里面放置block wx:for 循环movies 图片数组，再次使用swiper-item 依次将item.url 图片地址输出，就可以看到我们的轮播图了。
` <swiper class="swiper" indicator-dots="true" autoplay="true" interval="5000" duration="2000">
    <block wx:for="{{movies}}" wx:key="key">
      <swiper-item>
        <image src="{{item.url}}" class="slide-image" mode="widthfix" />
      </swiper-item>
    </block>
  </swiper> 
  `
 
####  [swiper 属性具体小提示](https://mp.weixin.qq.com/debug/wxadoc/dev/component/swiper.html)

🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇🎇
  
 ### 两个小动画： 加购小车左右摇摆动画效果， 收藏小爱心动画效果。
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/img/2.jpg)
 #### 点击图中的购买图片，我们的购物车就会随之扭扭身体，表示已经加入购物车☺ ， 具体代码如下：
 
 ####  在 wxml 中添加 animation="{{rorateAnimation}}"  动画。
 ####  [animation 动画](https://mp.weixin.qq.com/debug/wxadoc/dev/api/apianimation.html)
 

 `
 <image src='../../assets/icons/shopping.png' animation="{{rorateAnimation}}">         </image>
    `
#### 在对应的 js 中将购物车加购动画具体实现, 当 goumai: function() 触发时，创建动画 wx.createAnimation() , 通过 animation.rotate().step() 实现加购中小车摇摆的过程，代码如下：

 #### [biadtap 事件](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/event.html)
 
`
 // 购买点击事件，触发时使购物车图片放大
  goumai: function (event) {
    var animation = wx.createAnimation({
      duration: 100,
    })
    //购物车旋转
    animation.rotate(30).step();
    animation.rotate(0).step();
    animation.rotate(-30).step();
    animation.rotate(0).step();
     this.setData({
      rorateAnimation: animation.export(),
     })
  },
`
 ### 送走了购物车的加购动画，下面为大家分享一个收藏动画， 爱心 ♥
 
 ### 类似之前的购物车动画，首先我们在 wxml 中绑定事件 bindtap='shoucang' ,同样使用  animation="{{enlargeAnimation}}"  获取动画效果， 代码如下：
 
 `
 <image src='../../assets/icons/shoucang.png' animation="{{enlargeAnimation}}"           bindtap='shoucang'></image>
 `
 ###  js 文件中同样使用 wx.createAnimation() 创建动画，animation.opacity(0.6).scale(0.9).step();//修改透明度,放大  
 
 `
  //收藏，动画放大效果
  shoucang: function (event) {
    var animation = wx.createAnimation({
      duration: 700,
    })
    //  图片放大
    animation.opacity(0.6).scale(0.9).step();//修改透明度,放大  
     this.setData({
      enlargeAnimation: animation.export()
    })
  },
 `
 
 ## 是不是觉得超级简单， 通过一个事件绑定 秀出你的神操作。
 
 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈 🎈
 
 ###  下面，该是严肃的时候了，以上只是简单的切页面，我们要把喜欢的东西加进购物车，保持头脑清醒，跟着我把购物车的逻辑理理清楚 📣
 
 
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/img/2.jpg)
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/img/1.png)
 
 ###   首先我们要在 easy-mock 上创建一份自己的数据，
 [easy-mock 点击进入](https://www.easy-mock.com/login)
 
 ####   通过 wx.request({url:"....."}) 获取你的easy-mock 中的数据， 代码示例如下：
 
 
 `onLaunch(options) {
    // 请求数据
    wx.request({
      url: 'https://www.easy-mock.com/mock/5a0d83939b049244f7526362/getUserInfo',
      success: (response) => {
        //console.log(response.data.data);
        // 通过  this.globalData.movies 获取easy-mock 中的 movies 数组，
         //  除了全局变量的 js, 其他 js 页面要获取数据，需要 const app = getApp() 定义， 
         // 才能使用全局中的数据
        this.globalData.movies = response.data.data.movies,
           this.globalData.img = response.data.data.img,
           this.globalData.goods = response.data.data.goods
         }
    })
  },
`
 
 ####   由于商品页面加入购物车然后要在购物车页面显示， 我额外的设置了一个全局数组 buy ,以便之后在购物车页面显示我所添加的物品详情 （图片， 价格 ，数量等）, 在 app.js 中代码如下：
 
 
 `
 globalData: {
     buy:[]
  }
 `
 
 
####  在商品页面中的 js 文件中通过  onLoad: function () 将数据从全局中获取
 
 `
  onLoad: function () {
     this.setData({
      movies: app.globalData.movies,
      goods: app.globalData.goods
    });
 `
 
 ####   在 wxml 中通过绑定事件  bindtap='buy' 将选中的商品放入我们的购物车页面
  
   `
  <image src='../../assets/icons/goumai.png' data-id="{{item.id}}" bindtap='buy'></image>
   `
      
 ####   在对应的 js 文件中具体实现 buy 事件代码如下：
 
 ` // 购买， 点击图片，购物车显示已加购
  buy: function (e) {
    for (var i = 0; i < this.data.goods.length;i++){
    //点击购买图片触发 buy 事件 ，其中的 item.id 具有唯一性， 将其传入函数中与原来的所有商品中的 id 相匹配， 如果存在，即把它 push 进新的数组 buy 中。
     if (e.currentTarget.dataset.id == this.data.goods[i].id) {
        app.globalData.buy.push(this.data.goods[i])
        console.log(app.globalData.buy)
        }
     }
  }, 
 `
 
 
  🔊 🔊 🔊 🔊 🔊
  
  
##   购物车已经放好了我们的宝贝，接下来要显示才可以， 继续我们的 js 数据征途 fighting!!!
  
###     清楚地明白我们要什么， 如： 商品图片， 名称， 价格， 但是为了实现数量的增减效果，需要额外设置  全选 allSelect: "circle",  数量 num: 0,  共计数额  count: 0
  
  ` data: {
    allSelect: "circle",
    num: 0,
    count: 0,
     },
  `
  

 ####  将全局数据  buy 数组添加到我们购物车的 js 页面中， 代码如下：
  
  [setData](https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxs/06datatype.html)
  
  
  `  onLoad: function () {
  //通过  onload 函数加载数据
     this.setData({
      buy: app.globalData.buy,
    });
    },
  `
 ####  计算商品数量， 代码如下：
  
  `//计算数量
  countNum: function () {
    var that = this
    遍历数组，把既选中的num加起来
    var newList = that.data.buy
    var allNum = 0
    for (var i = 0; i < newList.length; i++) {
      if (newList[i].select == "success") {
        allNum += parseInt(newList[i].num)
      }
    }
    parseInt
    console.log(allNum)
    that.setData({
      num: allNum
    })
  },
  `
  
 ####  计算商品金额，代码如下：
  
  `//计算金额方法
  count: function () {
    var that = this
     选中的订单，数量*价格加起来
    var newList = that.data.buy
    var newCount = 0
    for (var i = 0; i < newList.length; i++) {
      if (newList[i].select == "success") {
        newCount += newList[i].num * newList[i].price
      }
    }
    that.setData({
      count: newCount
    })
  } `
   
 ####   将购物车中的商品数量添加，代码如下：
 
`<button class="goods-btn btn-add" data-index="{{index}}" data-num="{{item.num}}" bindtap="addtion">+</button>`
   
 `//商品数量增加函数
  addtion: function (e) {
    var that = this
    得到下标
    var index = e.currentTarget.dataset.index
     得到点击的值
    var num = e.currentTarget.dataset.num
       num++
     把新的值给新的数组
    var newList = that.data.buy
    newList[index].num = num
     把新的数组传给前台
    that.setData({
      buy: newList
    })
    调用计算数目方法
    that.countNum()
    计算金额
    that.count()
  }, `
  
 #### 将购物车中的商品数量减少，但数量减少到小于1时，该商品就会消失，代码如下：
 
`<button class="goods-btn btn-minus" data-index="{{index}}" data-num="{{item.num}}" bindtap="subtraction">—</button>`

  
 `商品数量减少
  subtraction: function (e) {
    var that = this
    得到下标
    var index = e.currentTarget.dataset.index
    得到点击的值
    var num = e.currentTarget.dataset.num
    把新的值给新的数组
    var newList = that.data.buy
    当1件时，点击移除
    if (num == 1) {
      newList.splice(index, 1)
    } else {
      num--
      newList[index].num = num
    }
   把新的数组传给前台
    that.setData({
      buy: newList
    })
    调用计算数目方法
    that.countNum()
    计算金额
    that.count()
  },`
 #### 单选商品，结算金额
 
`<icon type="{{item.select}}" size="26" data-index="{{index}}" data-select="{{item.select}}" bindtap="change" />`

   
 `change: function (e) {
     var that = this
    得到下标
     var index = e.currentTarget.dataset.index
     得到选中状态
    var select = e.currentTarget.dataset.select
    console.log(e.currentTarget.dataset.select);
    if (select == "circle") {
     var stype = "success"
    } else {
      var  stype = "circle"
    }
    把新的值给新的数组
     var newList = that.data.buy
     newList[index].select = stype
    把新的数组传给前台
    that.setData({
      buy: newList
    })
    调用计算数目方法
    that.countNum()
    计算金额
    that.count()
  },`

### 全选商品，并结算金额
 
`<view class="allSelect">
    <icon type="{{allSelect}}" size="26" data-select="{{allSelect}}" bindtap="allSelect" />
    <view class="allSelect-text">全选</view>
  </view> `
  
 `//全选
  allSelect: function (e) {
    var that = this
    //先判断现在选中没
    var allSelect = e.currentTarget.dataset.select
    var newList = that.data.buy
    if (allSelect == "circle") {
      //先把数组遍历一遍，然后改掉select值
      for (var i = 0; i < newList.length; i++) {
        newList[i].select = "success"
      }
      var select = "success"
    } else {
      for (var i = 0; i < newList.length; i++) {
        newList[i].select = "circle"
      }
      var select = "circle"
    }
    that.setData({
      buy: newList,
      allSelect: select
    })
    //调用计算数目方法
    that.countNum()
    //计算金额
    that.count()
  },
 `

#   ** 希望有兴趣的朋友一起讨论，一起切磋，欢迎交流 QQ：（1920459934）🙃 **
# [欢迎进入我的github浏览 🙈 ](https://github.com/AutumnTale666/miaomiaoXiong.github.io)  
