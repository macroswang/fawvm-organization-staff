<template>
  <div id="app">
    <FawvmOrganizationStaff
      :tree-show-icon="true"
      :tree-show-line="false"
      :tree-data-source="treeDataSource"
      :table-columns="tableClumns"
      :table-data-source="tableDataSource"
      @onTreeSelect="onTreeSelect"
      @handleSearch="handleSearch"
      @handleSelected="handleSelected"
      @onSelectChange="onSelectChange"
      @paginationChang="paginationChange"
    />
  </div>
</template>

<script>
import FawvmOrganizationStaff from "./components/FawvmOrganizationStaff";
const tableClumns = [
  {
    title: "姓名",
    dataIndex: "name",
    width: 120,
    align: "center"
  },
  {
    title: "邮箱",
    dataIndex: "email",
    width: 220,
    align: "center"
  },
  {
    title: "部门科室",
    dataIndex: "deptName",
    width: 150,
    align: "center"
  },
  {
    title: "岗位名称",
    dataIndex: "vwdutyName",
    width: 150,
    align: "center"
  }
];
const treeData = [
  {
    title: "一汽-大众汽车有限公司",
    key: "0-0",
    slots: {
      icon: "carry-out"
    },
    children: [
      {
        title: "总经理系统",
        key: "0-0-0",
        slots: {
          icon: "carry-out"
        },
        children: [
          {
            title: "管理服务部[P-MS]",
            key: "0-0-0-0",
            children: [
              { title: "管理服务部", key: "0-0-0-0-0" },
              { title: "总经理办公室", key: "0-0-0-0-1" },
              { title: "质量保证", key: "0-0-0-0-2" },
              { title: "采购[P-S]", key: "0-0-0-0-3" },
              { title: "法律风险合规部[P-LRC]", key: "0-0-0-0-4" }
            ]
          },
          { title: "总经理办公室[P-CO]", key: "0-0-0-1" },
          { title: "质量保证[P-Q]", key: "0-0-0-2" },
          { title: "采购", key: "0-0-0-3" },
          { title: "法律风险合规部", key: "0-0-0-4" }
        ]
      },
      {
        title: "第一副总经理(财务)系统[F]",
        key: "0-0-1",
        slots: {
          icon: "carry-out"
        },
        children: [
          { title: "0-0-1-1", key: "0-0-1-1" },
          { title: "0-0-1-2", key: "0-0-1-2" }
        ]
      },
      {
        title: "副总经理(人事)系统[H]",
        key: "0-0-2",
        slots: {
          icon: "carry-out"
        },
        children: [
          { title: "0-0-1-1", key: "0-0-2-1" },
          { title: "0-0-1-2", key: "0-0-2-2" }
        ]
      },
      {
        title: "副总经理(技术)系统[T]",
        key: "0-0-3",
        slots: {
          icon: "carry-out"
        },
        children: [
          { title: "0-0-1-1", key: "0-0-3-1" },
          { title: "0-0-1-2", key: "0-0-3-2" }
        ]
      },
      {
        title: "副总经理(商务)系统[C]",
        key: "0-0-4",
        slots: {
          icon: "carry-out"
        },
        children: [
          { title: "0-0-4-1", key: "0-0-4-1" },
          { title: "0-0-4-2", key: "0-0-4-2" }
        ]
      },
      {
        title: "奥迪销售[AUDI SALES]",
        key: "0-0-5",
        slots: {
          icon: "carry-out"
        },
        children: [
          { title: "0-0-1-1", key: "0-0-5-1" },
          { title: "0-0-1-2", key: "0-0-5-2" }
        ]
      },
      {
        title: "党委/纪委/工会[U]",
        key: "0-0-6",
        slots: {
          icon: "carry-out"
        },
        children: [
          { title: "0-0-1-1", key: "0-0-6-1" },
          { title: "0-0-1-2", key: "0-0-6-2" }
        ]
      },
      {
        title: "一汽-大众销售责任有限公司[S]",
        key: "0-0-7",
        slots: {
          icon: "carry-out"
        },
        children: [
          { title: "0-0-1-1", key: "0-0-7-1" },
          { title: "0-0-1-2", key: "0-0-7-2" }
        ]
      },
      {
        title: "成都分公司[CHENGDU]",
        key: "0-0-8",
        slots: {
          icon: "carry-out"
        },
        children: [
          { title: "0-0-1-1", key: "0-0-8-1" },
          { title: "0-0-1-2", key: "0-0-8-2" }
        ]
      }
    ]
  }
];
const tableDataSource = [
  {
    name: "王国科",
    email: "macrwang@deloitte.com.cn",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    name: "李冬祥",
    email: "dongxiang@deloitte.com.cn",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    name: "秦晓昆",
    email: "xiaokun@deloitte.com.cn",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    name: "何川",
    email: "hechuang@deloitte.com.cn",
    deptName: "技术部",
    vwdutyName: "后端"
  }
];
export default {
  name: "app",
  components: {
    FawvmOrganizationStaff
  },
  data() {
    return {
      tableClumns,
      tableDataSource,
      treeDataSource: treeData,
      msg: "Welcome to Your Vue.js App"
    };
  },
  created() {},
  methods: {
    handleSearch(val) {
      console.log("搜索的内容:", val);
    },
    handleSelected(selectedRows) {
      if (selectedRows && selectedRows.length === 0) {
        return this.$message.error("请选择行数据！");
      }
      console.log("确认表格中选择的内容:", selectedRows);
    },
    onSelectChange(selectedRow, selectedRowKey) {
      console.log("当前点击选择的内容:", selectedRow, selectedRowKey);
    },
    // 菜单选择
    onTreeSelect(selectedKeys, info) {
      console.log("selected", selectedKeys, info);
      console.log(info.node.title);
    },
    paginationChange(e) {
      console.log("分页的内容:", e);
    }
  }
};
</script>

<style lang="less" scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
