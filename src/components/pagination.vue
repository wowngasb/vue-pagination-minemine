<style lang="less">
.page-wrap {
  text-align: center;
  font-size: 18px;
  margin-top: 180px;

  ul {
    display: inline-block;
    list-style: none;
    overflow: hidden;

    li {
      float: left;
      color: #1e5a6b;
      padding: 1px 10px;
      margin: 0 5px;
      border-radius: 50%;
      user-select: none;
      border: 1px solid transparent;
    }
  }

  .pointer {
    cursor: pointer;
  }

  .hover {
    &:hover {
      border-color: #7ba6b3;
    }
  }

  .li-page {
    line-height: 1.5;
    cursor: pointer;
    color: #1e5a6b;

    &:hover {
      color: #7ba6b3;
    }
  }

  .active {
    border-color: #246c81;
  }
}
</style>

<template>
<div class="page-wrap">
  <ul v-show="prePage" class="li-page" @click="goPrePage">上一页</ul>
  <ul>
    <li v-for="i, index in showPageBtn" :key="index" :class="{active: i === currentPage, pointer: i, hover: i && i !== currentPage}"
        @click="pageOffset(i)">
      <a v-if="i" class="notPointer">{{i}}</a>
      <a v-else>···</a>
    </li>
  </ul>
  <ul v-show="nextPage" class="li-page" @click="goNextPage">下一页</ul>
</div>
</template>

<script>
export default {
  props: {
    value: {
      type: Number,
      default: 0
    },
    num: {
      type: [Number, String],
      default: 0
    },
    limit: {
      type: [Number, String],
      default: 1
    }
  },

  computed: {
    prePage() {
      return this.value > 1 && this.num;
    },

    nextPage() {
      return this.value < this.totalPage && this.num;
    },

    totalPage() {
      return Math.ceil(this.num / this.limit);
    },

    currentPage() {
      return this.value;
    },

    showPageBtn() {
      const pageNum = this.totalPage;
      const index = this.currentPage;
      if (pageNum <= 5) return [...new Array(5)].map((v, i) => i + 1);
      if (index <= 2) return [1, 2, 3, 0, pageNum];
      if (index >= pageNum - 1)
        return [1, 0, pageNum - 2, pageNum - 1, pageNum];
      if (index === 3) return [1, 2, 3, 4, 0, pageNum];
      if (index === pageNum - 2)
        return [1, 0, pageNum - 3, pageNum - 2, pageNum - 1, pageNum];
      return [1, 0, index - 1, index, index + 1, 0, pageNum];
    }
  },

  methods: {
    pageOffset(i) {
      if (i === 0 || i === this.currentPage) return;
      this.$emit("input", i);
    },

    goPrePage() {
      this.$emit("input", this.currentPage > 0 ? this.currentPage - 1 : 1);
    },

    goNextPage() {
      this.$emit(
        "input",
        this.currentPage < this.totalPage
          ? this.currentPage + 1
          : this.totalPage
      );
    }
  }
};
</script>
