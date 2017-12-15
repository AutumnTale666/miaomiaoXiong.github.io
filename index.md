 _Welcome to miaomiaoXiong's web blog_
 # 微信小程序之--（粉色冬天与唯品会的一场邂逅）
 买买买，虽然双十二刚过，可是唯品会的折扣却是依然火爆。一打开页面，便是粉色的主页映入眼帘，琳琅满目的商品，让我这个月光族过了把眼瘾。虽然买不起，那就自己模仿着写一个，把喜欢的商品一个个推进小推车里。（😍）下面为大家分享一个把唯品会里面的精致商品推进自己购物车的微信小程序，🙈
 
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/luping.gif)
 
## 在上面的录屏演示中，主要是实现购物车功能，话不多说，扔个代码看看：
 
### 主页面: 轮播图片这个小技巧比较普遍的被使用，代码如下：
 
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/img/4.jpg)
 
 #### index.wxml:
 ` <swiper class="swiper" indicator-dots="true" autoplay="true" interval="5000" duration="2000">
    <block wx:for="{{movies}}" wx:key="key">
      <swiper-item>
        <image src="{{item.url}}" class="slide-image" mode="widthfix" />
      </swiper-item>
    </block>
  </swiper> `
 
 使用swiper组件,滑块视图容器。
 
 
indicator-dots	Boolean	false	是否显示面板指示点	

autoplay	Boolean	false	是否自动切换	

interval	Number	5000	自动切换时间间隔	

duration	Number	500	滑动动画时长	
  
 ### 两个小动画： 加购小车左右摇摆动画效果， 收藏小爱心动画效果
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/img/2.jpg)
 #### bali/index.html:
  ` <viewclass="tui-bd__image">
    <imagedata-id="1"src="../../assets/icons/goumai.png"></image>
    </view>
    <view class='page__ft'>
     <view class='page-first__img' bindtap='onTap'>
       <image src='../../assets/icons/shouye.png'></image>
       </view>
      <view class='page-first__img'>
       <image src='../../assets/icons/shoucang.png' animation="{{enlargeAnimation}}"           bindtap='shoucang'></image>
     </view>
  <view class='page-first__img' bindtap='onTa'>
       <image src='../../assets/icons/shopping.png' animation="{{rorateAnimation}}">          </image>
     </view>
     </view> `

#### bali/index.js:
// 购买， 点击图片，购物车显示已加购
  ** buy: function (e) {
    for (var i = 0; i < this.data.goods.length;i++){
      if (e.currentTarget.dataset.id == this.data.goods[i].id) {
        app.globalData.buy.push(this.data.goods[i])
        console.log(app.globalData.buy)
  }
  }
},  

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
  }, **
 
 
 ### 最麻烦的当属购物车加购事件最麻烦了，给我一首歌的时间，且听我慢慢跟你说
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/img/1.png)
 #### sCar/sCar.wxml:
 `
 <view class="cart-box">
        <view class="item" wx:for="{{buy}}" wx:key="id">
                  <icon type="{{item.select}}" size="26" data-index="{{index}}" data-select="{{item.select}}" bindtap="change" />
                    <image src="{{item.url}}" class="goods-img" mode="widthFix"></image>
          <view class="goods-info">
                    <view class="row">
                      <view class="goods-name">{{item.name}}</view>
                      <text class="goods-price">￥{{item.price}}</text>
                    </view>
                    <!-- <view class="goods-type">
                      {{item.style}}
                    </view> -->
                    <view class="num-box">
                            <view class="btn-groups">
                              <button class="goods-btn btn-minus" data-index="{{index}}" data-num="{{item.num}}" bindtap="subtraction">—</button>
                              <view class="num">{{item.num}}</view>
                              <button class="goods-btn btn-add" data-index="{{index}}" data-num="{{item.num}}" bindtap="addtion">+</button>
                            </view>
                    </view>
          </view>
      </view>
    </view>
<view class="cart-fixbar">
        <view class="allSelect">
          <icon type="{{allSelect}}" size="26" data-select="{{allSelect}}" bindtap="allSelect" />
          <view class="allSelect-text">全选</view>
        </view>
        <view class="count">
          <text>合计：￥</text>{{count}}
        </view>
      <view class="order">
        去结算
        <text class="allnum">({{num}})</text>
      </view>
</view>
 `
  #### sCar/sCar.js:
  
 `
 const app = getApp()
Page({
  data: {
    allSelect: "circle",
    num: 0,
    count: 0,
     },
  onLoad: function () {
      this.setData({
      buy: app.globalData.buy,
    });
    },
    `
    `
  change: function (e) {
     var that = this
    //得到下标
     var index = e.currentTarget.dataset.index
     //得到选中状态
    var select = e.currentTarget.dataset.select
    console.log(e.currentTarget.dataset.select);
    if (select == "circle") {
     var stype = "success"
    } else {
      var  stype = "circle"
    }
    //把新的值给新的数组
     var newList = that.data.buy
     newList[index].select = stype

    //把新的数组传给前台
    that.setData({
      buy: newList
    })
    //调用计算数目方法
    that.countNum()
    //计算金额
    that.count()
  },
  `
  `
  //加法
  addtion: function (e) {
    var that = this
    //得到下标
    var index = e.currentTarget.dataset.index
     //得到点击的值
    var num = e.currentTarget.dataset.num
      //默认99件最多
    if (num < 100) {
      num++
    }
    //把新的值给新的数组
    var newList = that.data.buy
    newList[index].num = num

    //把新的数组传给前台
    that.setData({
      buy: newList
    })
    //调用计算数目方法
    that.countNum()
    //计算金额
    that.count()
  },
  `
  `
  // //减法
  subtraction: function (e) {
    var that = this
    //得到下标
    var index = e.currentTarget.dataset.index
    //得到点击的值
    var num = e.currentTarget.dataset.num
    //把新的值给新的数组
    var newList = that.data.buy
    //当1件时，点击移除
    if (num == 1) {
      newList.splice(index, 1)
    } else {
      num--
      newList[index].num = num
    }

    //把新的数组传给前台
    that.setData({
      buy: newList
    })
    //调用计算数目方法
    that.countNum()
    //计算金额
    that.count()
  },
 
  
  //全选
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
 
  // //计算数量
  countNum: function () {
    var that = this
    //遍历数组，把既选中的num加起来
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
  
  //计算金额方法
  count: function () {
    var that = this
    //思路和上面一致
    //选中的订单，数量*价格加起来
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
  }
})`
  
 

 

### 未完待续🙃

[欢迎来到me的github](https://github.com/AutumnTale666/miaomiaoXiong.github.io)  
