### vue倒计时组件

组件文件名称为`CountDown.vue`，对应的目录路径：`src/views/components/CountDown.vue`

```vue
<template>
  <div>
    <slot v-if="format" :time="formatCountdown(timeDiff)"></slot>
    <slot v-else :time="timeDiff"></slot>
  </div>
</template>

<script>
export default {
  name: 'CountDown',
  props: {
    time: {
      type: Number,
      default: 0
    },
    step: {
      //步进时间
      type: Number,
      default: 1
    },
    format:{
      //显示的倒计时格式
      type: String,
      default: 'mm : ss'
    }
  },
  data() {
    return {
      timer: '',
      timeDiff:0
    }
  },
  watch: {
    time(newVal){
      this.timeDiff = newVal
    }
  },
  mounted() {
  },
  beforeDestroy() {
    this.clearTimer()
  },
  methods: {
    triggerTimer() {
      this.timer = setTimeout(() => {
        this.timeDiff--
        // console.log(this.timeDiff)
        if (this.timeDiff <= 0) {
          this.stop()
          this.$emit('on-end')
        } else {
          this.$emit('on-countdown', this.timeDiff)
          this.triggerTimer()
        }
      }, this.step * 1000)
    },
    start(){
      this.timeDiff = this.time
      this.clearTimer()
      this.triggerTimer()
    },
    stop() {
      this.clearTimer()
    },
    clearTimer(){
      if (this.timer) clearTimeout(this.timer)
    },
    formatCountdown(timeDiff) {
      // 获取还剩多少小时
      const hour = parseInt(((timeDiff / 60 / 60).toString()))
      // 获取还剩多少分钟
      let minute = 0
      if (this.format.includes('hh') || this.format.includes('HH')) {
        minute = parseInt((((timeDiff  / 60) % 60).toString()))
      } else {
        minute = parseInt(((timeDiff  / 60).toString()))
      }
      // 获取还剩多少秒
      let second = 0
      if (this.format.includes('mm') || this.format.includes('mm')) {
        second = timeDiff  % 60
      } else {
        second = timeDiff
      }
      let result = this.format
      result = result.replace(/(hh|HH)/g, this.paddingZero(hour))
      result = result.replace(/(mm|MM)/g, this.paddingZero(minute))
      result = result.replace(/(ss|ss)/g, this.paddingZero(second))
      return result
    },
    paddingZero(val) {
      if (val <= 0) {
        return '00'
      } else if (val < 10) {
        return `0${val}`
      } else {
        return val.toString()
      }
    }
  },
}
</script>

```

#### 调用案例

```vue
<template>
    <count-down ref="countdown" :time="600" format="mm : ss" @on-end="onCountdownEnd">
        <template slot-scope="{ time }">{{ time }}</template>
    </count-down>
</template>
<script>
    import CountDown from "@/views/components/CountDown";//对应上面组件的文件路径
    export default {
    	components: {
            CountDown
        },
        mounted() {
            this.$refs['countdown'].start()
        },
        methods：{
        	 onCountdownEnd() {
                 console.log("倒计时结束")
             }
	    }
    }
</script>
```

