<template>
  <section>
    <ElDropDownTable
      v-model="input"
      placeholder="搜索"
      clearable
      :data="tableData"
      :total-page="totalPage"
      :ctrl="false"
      @next="next"
      @prev="prev"
      @query="query"
      @enter="enter"
      @current-row-changed="currentRowChanged"
    >
      <template>
        <el-table
          ref="table"
          :data="tableData"
          highlight-current-row
          style="width: 100%"
          size="mini"
          max-height="400"
        >
          <el-table-column type="index" width="50"> </el-table-column>
          <el-table-column prop="date" label="日期" width="180">
          </el-table-column>
          <el-table-column prop="name" label="姓名" width="180">
          </el-table-column>
          <el-table-column prop="address" label="地址"> </el-table-column>
        </el-table>
        <el-pagination
          background
          layout="prev, pager, next"
          :page-size="page.pageSize"
          :current-page="page.pageNum"
          :total="total"
        >
        </el-pagination>
      </template>
    </ElDropDownTable>
  </section>
</template>

<script>
import ElDropDownTable from "./el-dropdown-table";

export default {
  name: "HelloWorld",
  components: {
    ElDropDownTable,
  },
  props: {},
  data() {
    return {
      input: "",
      page: {
        pageSize: 10,
        pageNum: 1,
      },
      total: 1000,
      // tableData:[],
      // tableData: [
      //   {
      //     date: "2016-05-02",
      //     name: "张三",
      //     address: "上海市普陀区金沙江路 1518 弄",
      //   },
      // ],
      tableData: [
        {
          date: "2016-05-02",
          name: "张三",
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-04",
          name: "李四",
          address: "上海市普陀区金沙江路 1517 弄",
        },
        {
          date: "2016-05-01",
          name: "王五",
          address: "上海市普陀区金沙江路 1519 弄",
        },
        {
          date: "2016-05-03",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1516 弄",
        },
        {
          date: "2016-05-03",
          name: "lqr",
          address: "上海市普陀区金沙江路 1516 弄",
        },
        {
          date: "2016-05-03",
          name: "zf",
          address: "上海市普陀区金沙江路 1516 弄",
        },
        {
          date: "2016-05-03",
          name: "cbs",
          address: "上海市普陀区金沙江路 1516 弄",
        },
        {
          date: "2016-05-03",
          name: "sf",
          address: "上海市普陀区金沙江路 1516 弄",
        },
        {
          date: "2016-05-03",
          name: "xt",
          address: "上海市普陀区金沙江路 1516 弄",
        },
      ],
    };
  },
  computed: {
    totalPage: {
      get() {
        return this.total / this.page.pageSize;
      },
    },
  },
  methods: {
    next({ pageNum }) {
      this.page.pageNum = pageNum;
    },
    prev({ pageNum }) {
      this.page.pageNum = pageNum;
    },
    query() {
      console.log("query");
    },
    enter({ row, rowIndex }) {
      console.table("enter", row);
    },
    currentRowChanged({ row, rowIndex }) {
      this.$refs["table"].setCurrentRow(row);
      this.scrollTo("table", rowIndex);
    },
    scrollTo(refName, rowIndex) {
      if (!refName || !this.$refs[refName] || rowIndex < 0) return;
      let vmEl = this.$refs[refName].$el;
      if (!vmEl) return;
      const rows = vmEl.querySelectorAll(".el-table__body tr");
      const targetTop = rows[rowIndex].getBoundingClientRect().top;
      const body = vmEl.querySelector(".el-table__body");
      const containerTop = body.getBoundingClientRect().top;
      const scrollParent = vmEl.querySelector(".el-table__body-wrapper");
      scrollParent.scrollTop = targetTop - containerTop;
    },
  },
};
</script>
<style scoped>
</style>
