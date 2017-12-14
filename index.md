 _Welcome to miaomiaoXiong's web blog_
 # 微信小程序之--（粉色冬天与唯品会的一场邂逅）
 买买买，虽然双十二刚过，可是唯品会的折扣却是依然火爆。一打开页面，便是粉色的主页映入眼帘，琳琅满目的商品，让我这个月光族过了把眼瘾。虽然买不起，那就自己模仿着写一个，把喜欢的商品一个个推进小推车里。（😍）下面为大家分享一个把唯品会里面的精致商品推进自己购物车的微信小程序，🙈
 
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/luping.gif)
 
## 在上面的录屏演示中，主要是实现购物车功能，话不多说，扔个代码看看：
 
### 主页面: 轮播图片这个小技巧比较普遍的被使用，代码如下：
 
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/img/4.jpg)
 
 #### wxml:
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
 
 #### wxml:
 ` <view class='tui-bd__image'>
         <image src='../../assets/icons/goumai.png' data-id="{{item.id}}" bindtap='buy'></image>
 </view>
 
 <view class='page__ft'>
  <view class='page-first__img' bindtap='onTap'>
    <image src='../../assets/icons/shouye.png'></image>

  </view>

  <view class='page-first__img'>
    <image src='../../assets/icons/shoucang.png' animation="{{enlargeAnimation}}" bindtap='shoucang'></image>
  </view>

  <view class='page-first__img' bindtap='onTa'>
    <image src='../../assets/icons/shopping.png' animation="{{rorateAnimation}}"></image>
  </view>
</view> `


#### wxjs:
// 购买， 点击图片，购物车显示已加购
  `buy: function (e) {
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
  },`
 
 
 ### 最麻烦的当属购物车加购事件最麻烦了，给我一首歌的时间，且听我慢慢跟你说
 
 
 
 
  
 
Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/AutumnTale666/miaomiaoXiong.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
