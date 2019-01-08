<template>
  <div class="marqueeBox" :style="{height: `${rowsNum * lineHeight}px`, overflow: 'hidden'}">
    <ul :style="translate">
      <li v-for="(item, index) in msgList" :style="{'line-height': `${lineHeight}px`}" :key="index">{{item}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    // 列表数据
    dataList: {
      type: Array,
      required: true
    },
    // 滚动方向
    direction: {
      type: String,
      default: "up"
    },
    // 单行的行高
    lineHeight:{
      default: 40,
    },
    // 翻滚时间
    interval:{
      default: 2000,
    },
    // 显示区域展示的条目
    rowsNum:{
      default: 1,
    }
  },
  data() {
    return {
      msgList: [],
      top: 0,
      translate: ""
    };
  },
  created() {
    // 可以循环滚动
    this.msgList.push(...this.dataList);
    this.dataList.forEach((item, index) => {
      index < this.rowsNum && this.msgList.push(item);
    });

    setInterval(() => {
      this.top -= this.lineHeight;
      this.translate = `transform: translate3d(0px, ${
        this.top
      }px, 0px); transition: transform 500ms ease 0s`;
      if (this.top <= `-${this.dataList.length * this.lineHeight}`) {
        setTimeout(() => {
          this.top = 0;
          this.translate = `transform: translate3d(0px, 0px, 0px)`;
        }, 500);
      }
    }, this.interval);
  }
};
</script>

<style scoped lang="less">
</style>



