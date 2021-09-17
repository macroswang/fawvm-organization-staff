<template>
  <div id="app">
    人员选择<a-input
      v-model="receiver"
      :disabled="false"
      placeholder="请输入"
      @click="handleOrganizationVisible"
    />
    <!-- 选择人员 -->
    <Modal :visible.sync="organizationVisible" contentStyle="width: 96%;">
      <FawvmOrganizationStaff
        :tree-show-icon="true"
        :tree-show-line="false"
        :tree-data-source="treeDataSource"
        :table-size="'small'"
        :tree-current-key="treeCurrentKey"
        :table-left-columns="tableLeftColumns"
        :table-right-columns="tableRightColumns"
        :target-keys="targetKeys"
        :table-data-source="tableDataSource"
        @onTreeSelect="onTreeSelect"
        @handleSearch="handleSearch"
        @handleSelected="handleSelected"
        @onSelectChange="onSelectChange"
        @handleReset="handleReset"
      />
    </Modal>
  </div>
</template>

<script>
import Modal from "./components/Modal";
import FawvmOrganizationStaff from "./components/FawvmOrganizationStaff";
const tableLeftColumns = [
  {
    title: "姓名",
    dataIndex: "title",
    ellipsis: true,
    align: "center"
  },
  {
    title: "邮箱",
    dataIndex: "email",
    ellipsis: true,
    align: "center"
  },
  {
    title: "部门科室",
    dataIndex: "deptName",
    ellipsis: true,
    align: "center"
  },
  {
    title: "岗位名称",
    dataIndex: "vwdutyName",
    ellipsis: true,
    align: "center"
  }
];
const tableRightColumns = [
  {
    title: "姓名",
    dataIndex: "title",
    ellipsis: true,
    align: "center"
  },
  {
    title: "邮箱",
    dataIndex: "email",
    ellipsis: true,
    align: "center"
  },
  {
    title: "部门科室",
    dataIndex: "deptName",
    ellipsis: true,
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
const tableDataSource1 = [
  {
    title: "小王",
    key: "macrwang1@xxx.com",
    email: "macrwang1@xxx.com",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    title: "小李",
    key: "xiaoli@xxx.com",
    email: "xiaoli@xxx.com",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    title: "小秦",
    key: "xiaoqin@xxx.com",
    email: "xiaoqin@xxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小何",
    key: "hexiao4@xxx.com",
    email: "hexiao4@xxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小周",
    key: "zhouxiao@xxx.com",
    email: "zhouxiao@xxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小张",
    key: "zhangxiao@xxx.com",
    email: "zhangxiao@xxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小杨",
    key: "yangxiao@xxx.com",
    email: "yangxiao@xxx.com",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    title: "小黄",
    key: "hangxiao@xxx.com",
    email: "hangxiao@xxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小马",
    key: "maxiao@xxx.com",
    email: "maxiao@xxx.com",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    title: "小胡",
    key: "huxiao@xxx.com",
    email: "huxiao@xxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小睿",
    key: "ruiiao@xxx.com",
    email: "ruiiao@xxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  }
];

const tableDataSource2 = [
  {
    title: "小王1",
    key: "macrwang1@axxx.com",
    email: "macrwang1@axxx.com",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    title: "小李2",
    key: "xiaoli@axxx.com",
    email: "xiaoli@axxx.com",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    title: "小秦3",
    key: "xiaoqin@axxx.com",
    email: "xiaoqin@axxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小何4",
    key: "hexiao4@axxx.com",
    email: "hexiao4@axxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小周5",
    key: "zhouxiao@axxx.com",
    email: "zhouxiao@axxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小张6",
    key: "zhangxiao@axxx.com",
    email: "zhangxiao@axxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小杨7",
    key: "yangxiao@axxx.com",
    email: "yangxiao@axxx.com",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    title: "小黄8",
    key: "hangxiao@axxx.com",
    email: "hangxiao@axxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小马9",
    key: "maxiao@axxx.com",
    email: "maxiao@axxx.com",
    deptName: "技术部",
    vwdutyName: "前端"
  },
  {
    title: "小胡10",
    key: "huxiao@axxx.com",
    email: "huxiao@axxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  },
  {
    title: "小睿12",
    key: "ruiiao@axxx.com",
    email: "ruiiao@axxx.com",
    deptName: "技术部",
    vwdutyName: "后端"
  }
];

export default {
  name: "app",
  components: {
    Modal,
    FawvmOrganizationStaff
  },
  data() {
    return {
      receiver: "",
      receivers: [],
      targetKeys: [],
      selectedRows: [],
      treeCurrentKey: "",
      tableLeftColumns,
      tableRightColumns,
      tableDataSource: [],
      organizationVisible: false, // 人员树
      treeDataSource: treeData
    };
  },
  created() {},
  methods: {
    handleSearch(val) {
      console.log("搜索的内容:", val);
    },
    handleReset() {
      this.receiver = "";
      this.targetKeys = [];
      this.selectedRows = [];
    },
    handleSelected(allSelectedRows) {
      if (allSelectedRows && allSelectedRows.length === 0) {
        return this.$message.error("请选择行数据！");
      }
      console.log("allSelectedRows=>", allSelectedRows);
      this.receiver = "";
      this.receivers = [];
      this.selectedRows = [];
      allSelectedRows.forEach(item => {
        let obj = {};
        obj.email = item.email;
        obj.userId = item.gid;
        obj.userName = item.name;
        this.receivers.push(obj);
        this.selectedRows.push(item);
      });
      this.receiver = this.receivers.map(item => item.userName).join(",");
      this.organizationVisible = false;
      this.targetKeys = [];
    },
    onSelectChange(selectedRow, selectedRowKey) {
      console.log("当前点击选择的内容:", selectedRow, selectedRowKey);
    },
    // 菜单选择
    onTreeSelect(selectedKeys, info) {
      // console.log("selected", selectedKeys, info);
      this.treeCurrentKey = selectedKeys[0];
      if (selectedKeys[0] === "0-0-0-1") {
        this.tableDataSource = tableDataSource1;
      } else if (selectedKeys[0] === "0-0-0-2") {
        this.tableDataSource = tableDataSource2;
      }
      console.log(info.node.title);
    },
    paginationChange(e) {
      console.log("分页的内容:", e);
    },
    handleOrganizationVisible() {
      this.organizationVisible = true;
      this.tableDataSource = [];
      this.treeCurrentKey = "";
      this.targetKeys = [...this.selectedRows];
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
// 修改hover的背景颜色ant-table-tbody
.ant-table-tbody {
  > tr:hover:not(.ant-table-expanded-row) > td,
  .ant-table-row-hover,
  .ant-table-row-hover > td {
    background: #e5f2ff !important;
  }
}
.ant-table-fixed {
  .ant-table-row-hover,
  .ant-table-row-hover > td {
    background: #e5f2ff !important;
  }
}
/deep/ .ant-table-tbody {
  max-height: 540px;
  overflow-y: scroll;
  overflow-x: hidden;
}
.ant-table-bordered .ant-table-thead > tr > th,
.ant-table-bordered .ant-table-tbody > tr {
  border-top: 0px solid #ffffff !important;
  border: 0px solid #fff;
}
</style>
