# translate模拟滚动

------

基于VUE实现的模拟滚动，(适合用于移动的webview)可提高性能

------


### 1. 设置可视区域
```html
<template>
  <div class='match-team-style'>
    <div class='base-data'>
      <p>ping1和ping3为可是区域，里面的内容可滚动</p>
      <div class='ping1'>
        <ul ref='u1' :style='movedist'>
          <li v-for='n in 100'>{{n}}</li>
        </ul>
      </div>
      <div class='ping2'>
        <ul>
          <li v-for='n in 100'>{{n}}</li>
        </ul>
      </div>
      <div class='ping3'>
        <ul ref='u3' :style='movedist3'>
          <li v-for='n in 100'>{{n}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>
```

```css
<style lang='less'>
	.match-team-style{
     border:1px solid red;
     width: 100%;
     height: 100%;
     >div{
      border:1px solid yellow;
      height: 100%;
      >div{
        margin-top: 100px;
        width:80px;
        border:1px solid blue;
        display: inline-block;
        height: 300px;
        overflow: hidden;
        ul{
          border: 1px solid red;
        }
      }
     }
	}
</style>
```
### 2. 通过监听touch事件来动态改变u1和u3的translate

```js
<script>
export default {
  components: {},
  data () {
    return {
      movedist: '',
      movedist3: ''
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.moveDiv(this.$refs.u1, this.$refs.u3)
    })
  },
  methods: {
    moveDiv (dom1, dom2) {
      // 滑动dom1,滑动dom2,dom1和dom2有连锁滑动效果
      // self.movedist、self.movedist3是dom1和2的style
      let startY1 = ''
      let moveY1 = ''
      let movedis = 0
      let translates = 0
      let self = this
      dom1.addEventListener('touchstart', function (e) {
        startY1 = e.changedTouches[0].pageY
      })
      dom1.addEventListener('touchmove', function (e) {
        moveY1 = e.changedTouches[0].pageY
        movedis = translates + moveY1 - startY1
        if (movedis > 0) {
          movedis = 0
        } else if (movedis < -(dom1.offsetHeight - 300)) {
          movedis = -(dom1.offsetHeight - 300)
        }
        self.movedist = `transform: translateY(${movedis}px)`
        self.movedist3 = `transform: translateY(${movedis}px)`
      })
      dom1.addEventListener('touchend', function (e) {
        translates = movedis
      })
      let startY3 = ''
      let moveY3 = ''
      dom2.addEventListener('touchstart', function (e) {
        startY3 = e.changedTouches[0].pageY
      })
      dom2.addEventListener('touchmove', function (e) {
        moveY3 = e.changedTouches[0].pageY
        movedis = translates + moveY3 - startY3
        if (movedis > 0) {
          movedis = 0
        } else if (movedis < -(dom1.offsetHeight - 300)) {
          movedis = -(dom1.offsetHeight - 300)
        }
        self.movedist = `transform: translateY(${movedis}px)`
        self.movedist3 = `transform: translateY(${movedis}px)`
      })
      dom2.addEventListener('touchend', function (e) {
        translates = movedis
      })
    }
  }
}
</script>
```
关键代码就是在监听touchmove里实时计算滑动的距离，来设置u1和u3的滚动距离。
