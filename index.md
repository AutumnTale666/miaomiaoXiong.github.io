 _Welcome to miaomiaoXiong's web blog_
 # 微信小程序之--（粉色冬天与唯品会的一场邂逅）
 买买买，虽然双十二刚过，可是唯品会的折扣却是依然火爆。一打开页面，便是粉色的主页映入眼帘，琳琅满目的商品，让我这个月光族过了把眼瘾。虽然买不起，那就自己模仿着写一个，把喜欢的商品一个个推进小推车里。（😍）下面为大家分享一个把唯品会里面的精致商品推进自己购物车的微信小程序，🙈
 
 ![Image](https://github.com/AutumnTale666/WEAPP_DEMO/blob/master/weiPH/luping.gif)
 
## 在上面的录屏演示中，主要是实现购物车功能，话不多说，扔个代码看看：
 
### 主页面: 轮播图片这个小技巧比较普遍的被使用，代码如下：
 
  <swiper class="swiper" indicator-dots="true" autoplay="true" interval="5000" duration="2000">
    <block wx:for="{{movies}}" wx:key="key">
      <swiper-item>
        <image src="{{item.url}}" class="slide-image" mode="widthfix" />
      </swiper-item>
    </block>
  </swiper>
 
 使用swiper组件,滑块视图容器。
 
 属性名	类型	默认值	说明	最低版本
indicator-dots	Boolean	false	是否显示面板指示点	
autoplay	Boolean	false	是否自动切换	
interval	Number	5000	自动切换时间间隔	
duration	Number	500	滑动动画时长	
 
 
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
