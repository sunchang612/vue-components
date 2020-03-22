<template>
  <div class="sc-pagination-container">
    <ul class="page_number-wrapper">
      <li 
        class="slide-number"
        :class="{active: currentPage === 1}"
        @click="go(1)"
        v-show="currentPage > 0">
          1
      </li>

      <li class="dots" v-show="showPrevMore">
        <span>...</span>
      </li>

      <li
        class="slide-number" 
        v-for="(num, key) in pageList"
        :key="key"
        :class="{active: currentPage === num}"
        @click="go(num)">
          {{ num }}
      </li>

      <li class="dots" v-show="showNextMore">
        <span>...</span>
      </li>

      <li 
        class="slide-number"
        :class="{active: currentPage === total}"
        @click="go(total)"
        v-show="total > 1">
          {{ total }}
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'pagination',

  props: {
    currentPage: {
      type: Number,
      default: 1,
      validator(val){ return val >= 0 }
    },
    total: {
      type: Number,
      validator(val){ return val >= 0 }
    }
  },

  data() {
    return {
      pageCount: 7,
      showPrevMore: false,
      showNextMore: false
    }
  },

  computed: {
    pageList() {
      const { currentPage, total, pageCount } = this
      let numAry = []
      let showPrevMore = false
      let showNextMore = false
      // 判断是否展示 icon
      if (total > pageCount) {
        if (currentPage > pageCount - 3) {
          showPrevMore = true
        }
        if (currentPage < total - 3) {
          showNextMore = true
        }
      }
      // 这里我说的上 表示 上按钮icon，下 表示 下按钮 icon
      // 当显示上 并不展示 下 
      if (showPrevMore && !showNextMore) {
        const startPage = total - (pageCount - 3)
        for(let i = startPage; i < total; i++) {
          numAry.push(i)
        }
      } else if (!showPrevMore && showNextMore) { // 右显示 
        for (let i = 2; i < pageCount - 1; i++) {
          numAry.push(i)
        }
      } else if (showPrevMore && showNextMore) { // 两边都展示
        const count = Math.floor(pageCount/2) - 2
        for (let i = currentPage - count; i <= currentPage + count; i++) {
          numAry.push(i)         
        }
      } else {
        for (let i = 2; i < total; i++) {
          numAry.push(i)
        }
      }
      this.showPrevMore = showPrevMore
      this.showNextMore = showNextMore
      return numAry
    }
  },

  methods: {
    go(page) {
      this.$emit('update:currentPage', Number(page));
    }
  }
}
</script>
<style lang="less" scoped>
.sc-pagination-container {
  .page_number-wrapper {
    display: inline-flex;
    .slide-number {
      padding:0 8px;
      line-height: 26px;
      border: 1px solid #ccc;
      border-radius: 3px;
      margin: 0 6px;
      cursor: pointer;
    }
    .active {
      background-color: #409eff;
      color: #fff;
    }
  }
}
</style>
