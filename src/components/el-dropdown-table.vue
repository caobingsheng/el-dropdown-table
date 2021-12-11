<template>
  <section>
    <el-popover
      ref="popover"
      placement="bottom"
      :width="tableWidth"
      trigger="manual"
      v-model="display"
    >
      <slot>[内容插槽,例如:表格]</slot>
    </el-popover>
    <el-input
      v-popover:popover
      v-bind="$attrs"
      v-on="$listeners"
      @focus="inputFocus"
      @keydown.native="inputKeydown"
      @blur="inputBlur"
    ></el-input>
  </section>
</template>

<script>
// 事件: 当前行改变, 可以在事件处理中滚动到当前行
const EVENT_CURRENT_ROW_CHANGED = "current-row-changed";
export default {
  name: "ElDropDownTable",
  props: {
    /**
     * 下拉表格的宽度
     */
    tableWidth: {
      type: String,
      default: "600",
    },
    /**
     * true: ctrl+左右方向键翻页
     * false: 方向键翻页
     */
    ctrl: {
      type: Boolean,
      default: true,
    },
    /**
     * 表格数据
     */
    data: {
      type: Array,
      default: () => [],
    },
    /**
     * 分页总页数
     */
    totalPage: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      display: false,
      currentRowIndex: -1,
      currentRow: null,
      currentPageNum: 1,
    };
  },
  methods: {
    inputFocus(e) {
      this.display = true;
    },
    inputKeydown(e) {
      switch (e.keyCode) {
        case 37:
          // 左
          if (!this.ctrl || (this.ctrl && e.ctrlKey)) {
            e.preventDefault();
            if (this.totalPage <= 0) {
              this.currentPageNum = 0;
            } else if (this.currentPageNum <= 1 && this.totalPage >= 1) {
              this.currentPageNum = 1;
            } else {
              this.currentPageNum = this.currentPageNum - 1;
            }
            // 通知翻上一页
            this.$emit("prev", {
              pageNum: this.currentPageNum,
            });
            this.resetRow();
          }
          break;
        case 38:
          // 上
          e.preventDefault();
          if (this.data && this.data.length) {
            if (this.currentRowIndex <= 0) {
              this.currentRowIndex = this.data.length - 1;
            } else {
              this.currentRowIndex = this.currentRowIndex - 1;
            }
            this.sendCurrentRow();
          }
          this.$emit("up", e);
          break;
        case 39:
          // 右
          if (!this.ctrl || (this.ctrl && e.ctrlKey)) {
            e.preventDefault();
            if (this.currentPageNum >= this.totalPage) {
              this.currentPageNum = this.totalPage;
            } else if (this.currentPageNum <= 0 && this.totalPage >= 1) {
              this.currentPageNum = 1;
            } else {
              this.currentPageNum = this.currentPageNum + 1;
            }
            // 通知翻下一页
            this.$emit("next", {
              pageNum: this.currentPageNum,
            });
            this.resetRow();
          }
          break;
        case 40:
          // 下
          e.preventDefault();
          if (this.data && this.data.length) {
            if (this.currentRowIndex === this.data.length - 1) {
              this.currentRowIndex = 0;
            } else {
              this.currentRowIndex = this.currentRowIndex + 1;
            }
            this.sendCurrentRow();
          }
          this.$emit("down", e);
          break;
        case 13:
          // 回车
          e.preventDefault();
          this.$emit("enter", {
            row: this.currentRow,
            rowIndex: this.currentRowIndex,
          });
          break;
        case 16:
        case 17:
        case 18:
          // ctrl shift alt
          break;
        default:
          this.resetRow();
          this.$emit("query", e);
          break;
      }
    },
    resetRow() {
      this.currentRowIndex = -1;
      this.sendCurrentRow();
    },
    sendCurrentRow() {
      if (this.data && this.currentRowIndex >= 0) {
        this.currentRow = this.data[this.currentRowIndex]; // 当前已选择行
      } else {
        this.currentRow = null;
      }
      this.$emit(EVENT_CURRENT_ROW_CHANGED, {
        row: this.currentRow,
        rowIndex: this.currentRowIndex,
      });
    },
    inputBlur(e) {
      this.display = false;
    },
  },
};
</script>
<style scoped>
</style>
