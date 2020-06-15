# 动态置顶栏

- 滚动时，监听scroll事件，获取到滚动距离
```js
    <div class="container" ref="container" @scroll='scroll'>
    </div>

    data() {
      return {
        scrollTop: -1
      }
    }
    methods: {
      scroll(ev) {
        this.scroll = ev.target.scrollTop
      }
    }

```
- 获取到每一项的clientHeight，保存到一个数组中
```js
    <div class="exercise-item" v-for='(item, i) in list' :key='item.id' ref="exerciseItem">
    </div>
    methods: {
      _listHeight() {
        // 定义数组存放每个item的高度
        this.listHeight = []
        // 获取到所有exerciseItem
        const listItem = this.$refs.exerciseItem
        // 定义初始高度
        let height = 0
        this.listHeight.push(height)
        // 遍历listItem， 获取每一项的clientHeight， push到数组中
        // [0, 283, 648, 1261, 1544, 1992]
      }
    }
```
- 通过watch
```js
// 监听data中的 list，初始状态为 []
// 在created中获取数据，赋值给list
watch: {
  list(newList) {
    setTimeout(() => {
      // 调用 _listHeight()
    })
  },
  // 关键点
  // 监听scrollTop
  scrollTop(newScrollTop) {
    if(newScrollTop < 0) {
      // currentIndex 控制当前pup层显示哪一项的内容
      this.currentIndex = 0
      return
    }
    // 中间滚动，通过判断当前滚动的距离 在listHeight的哪个区间范围，来确定currentIndex
    for(let i=0; i<this.listHeight.length-1; i++) {
      // 注意是length-1
      // 取出第 i 和 i+1
      let height1 = this.listHeight[i]
      let height2 = this.listHeight[i+1]
      // 
      if(newScrollTop >= height1 && newScrollTop <= height2) {
        // 确定 currentIndex
        this.currentIndex = i
        // data中定义可一个控制pup动效的变量
        this.diff =  height2 - newScorllTop
      }
    }

  },
  // 
  diff(newDiff) {
    console.log('newDiff---', newDiff)
    // 设置动效
    let pupTop = (newDiff > 0 && newDiff < this.pupHeight)? newDiff-this.pupHeight : 0
    let opacityPercent = (newDiff > 0 && newDiff < this.pupHeight)? Math.abs(newDiff) / this.pupHeight : 1
    this.$refs.pup.style.transform = `translate3d(0,${pupTop}px,0)`
    this.$refs.pup.style.opacity = opacityPercent
    this.$refs.pup.style.transition = `all 0.2s`
  }
}

```

