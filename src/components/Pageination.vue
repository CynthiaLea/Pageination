<template>
  <div class="pageination">
    <!-- 当前页是1的时候，禁用 -->
    <button
      :disabled="myCurrentPage === 1"
      :class="{ disabled: myCurrentPage === 1 }"
      @click="setCurrentPage(myCurrentPage - 1)"
    >
      上一页
    </button>
    <button v-if="startEnd.start > 1"  @click="setCurrentPage(1)">1</button>
    <button class="disabled" v-if="startEnd.start > 2">...</button>
    <button
      v-for="item in startEndArr"
      :key="item"
      :class="{ active: item === myCurrentPage }"
      @click="setCurrentPage(item)"
    >
      {{ item }}
    </button>
    <button class="disabled" v-if="startEnd.end < totalPage - 1">...</button>
    <button v-if="startEnd.end < totalPage" @click="setCurrentPage(totalPage)">
      {{ totalPage }}
    </button>
    <button
      :disabled="myCurrentPage === totalPage"
      :class="{ disabled: myCurrentPage === totalPage }"
      @click="setCurrentPage(myCurrentPage + 1)"
    >
      下一页
    </button>
    <button class="disabled">共 {{ total }} 条</button>
  </div>
</template>

<script>
export default {
  name: "Pageination",
  props: {
    currentPage: {
      type: Number,
      default: 1,
    },
    total: {
      type: Number,
      default: 0,
    },
    pageSize: {
      type: Number,
      default: 10,
    },

    showPageNo: {
      type: Number,
      default: 5,
      validator: function (value) {
        return value % 2 === 1;
      },
    },
  },

  data() {
    return {
      myCurrentPage: this.currentPage,
    };
  },

  computed: {
    totalPage() {
      const { total, pageSize } = this;
      return Math.ceil(total / pageSize);
    },

    startEnd() {
      const { myCurrentPage, showPageNo, totalPage } = this;
      let start, end;
      start = myCurrentPage - Math.floor(showPageNo / 2);
      if (start < 1) {
        start = 1;
      }

      end = start + showPageNo - 1;

      if (end > totalPage) {
        // 修改end为totalPage
        end = totalPage;
        // reset start
        start = end - showPageNo + 1;
        if (start < 1) {
          start = 1;
        }
      }
      return { start, end };
    },

    startEndArr(){
      const arr = []
      const {start, end} = this.startEnd
      for(let page = start; page <= end; page++){
        arr.push(page)
      }

      return arr
    }
  },

  methods: {
    // 切换页码
    setCurrentPage(page) {
      // 如果页码没有改变，依然点击了，直接return
      if (page === this.myCurrentPage) return;
      // 如果页码变了，将传递过来的page赋值给myCurrentPage 
      this.myCurrentPage = page;
      // 分发自定义事件，告诉父组件
      this.$emit("currentChange", page);
    },
  },

  watch: {
    /**
     * 监视父组件传来的页码。同步更新页码
     */
    currentPage(value) {
      this.myCurrentPage = value;
    },
  },
};
</script>

<style lang="scss" scoped>
.pageination {
  button {
    margin: 0 5px;
    background-color: #f4f4f5;
    color:#009a61;
    outline: none;
    border-radius: 2px;
    pad: 0 4px;
    vertical-align: top;
    display: inline-block;
    font-size: 13px;
    min-width: 35.5px;
    height: 28px;
    line-height: 28px;
    cursor: pointer;
    box-sizing: border-box;
    text-align: center;
    border: 0;

    &.active {
      background-color: #009a61;
      // border-color: #009a61;
      color: #fff;
      cursor: not-allowed;
    }

    &.disabled {
      cursor: not-allowed;
      color: #ccc;
    }
  }
}
</style>