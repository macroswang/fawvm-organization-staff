<template>
  <div class="container">
    <div class="leftContainer">
      <a-tree
        :show-icon="treeShowIcon"
        :show-line="treeShowLine"
        :tree-data="treeDataSource"
        @select="onTreeSelect"
      >
        <a-icon slot="icon" type="carry-out" />
        <a-tree-node key="0-0">
          <a-icon slot="icon" type="carry-out" />
          <span slot="title" style="color: #1890ff">一汽-大众汽车有限公司</span>
          <a-tree-node key="0-0-0" title="总经理系统">
            <a-icon slot="icon" type="carry-out" />
            <a-tree-node key="0-0-0-0" title="leaf">
              <a-icon slot="icon" type="carry-out" />
            </a-tree-node>
            <a-tree-node key="0-0-0-1" title="leaf">
              <a-icon slot="icon" type="carry-out" />
            </a-tree-node>
            <a-tree-node key="0-0-0-2" title="leaf">
              <a-icon slot="icon" type="carry-out" />
            </a-tree-node>
          </a-tree-node>
          <a-tree-node key="0-0-1" title="第一副总经理(财务)系统[F]">
            <a-icon slot="icon" type="carry-out" />
            <a-tree-node key="0-0-1-0" title="leaf">
              <a-icon slot="icon" type="carry-out" />
            </a-tree-node>
          </a-tree-node>
          <a-tree-node key="0-0-2" title="副总经理(人事)系统[H]">
            <a-icon slot="icon" type="carry-out" />
            <a-tree-node key="0-0-2-0" title="leaf">
              <a-icon slot="icon" type="carry-out" />
            </a-tree-node>
            <a-tree-node key="0-0-2-1" title="leaf">
              <a-icon slot="icon" type="carry-out" />
              <a-icon slot="switcherIcon" type="form" />
            </a-tree-node>
          </a-tree-node>
        </a-tree-node>
      </a-tree>
    </div>
    <div class="rightContainer">
      <div class="top-right-container">
        <div class="search">
          <a-input
            v-model="partNo"
            style="width:250px"
            placeholder="请输入姓名\工号\账号"
          />
          <a-button class="search-button" type="primary" @click="handleSearch"
            >搜索</a-button
          >
        </div>
        <div class="selectedConfirm">
          <a-button class="search-button" type="primary" @click="handleSelected"
            >确定选择</a-button
          >
        </div>
      </div>

      <a-table
        :row-key="(record, index) => index"
        :columns="tableColumns"
        :row-selection="rowSelection"
        :data-source="tableDataSource"
        :pagination="pagination"
        @change="paginationChange"
      >
      </a-table>
    </div>
  </div>
</template>
<script>
export default {
  name: "FawvmOrganizationStaff",
  data() {
    return {
      partNo: "",
      selectedRows: []
    };
  },
  props: {
    treeShowIcon: {
      type: Boolean,
      default: true
    },
    treeShowLine: {
      type: Boolean,
      default: true
    },
    treeDataSource: {
      type: Array,
      default: () => []
    },
    tableColumns: {
      type: Array,
      default: () => []
    },
    tableDataSource: {
      type: Array,
      default: () => []
    },
    hasRowSelection: {
      type: Boolean,
      default: true
    },
    selectedRowKeys: {
      type: Array,
      default: () => []
    },
    pagination: {
      type: Object,
      default: () => {
        return {
          current: 1,
          total: 0, // 每页中显示数据的条数
          pageSize: 10, // 每页中显示5条数据
          showSizeChanger: true,
          pageSizeOptions: ["10", "20", "50", "100"], // 每页中显示的数据
          showTotal: total => `共有 ${total} 条数据` // 分页中显示总的数据
        };
      }
    }
  },
  watch: {
    tableDataSource(val) {
      console.log("Watch>>>>>>>tableDataSource=", val);
    }
  },
  computed: {
    rowSelection() {
      return this.hasRowSelection
        ? {
            onChange: this.onSelectChange,
            getCheckboxProps: record => {
              return {
                props: {
                  defaultChecked: this.selectedRowKeys.includes(record.id)
                }
              };
            }
          }
        : null;
    }
  },
  methods: {
    handleSearch() {
      this.$emit("handleSearch", this.partNo);
    },
    handleSelected() {
      this.$emit("handleSelected", this.selectedRows);
    },
    onTreeSelect(selectedKeys, info) {
      this.$emit("onTreeSelect", selectedKeys, info);
    },
    onSelectChange(selectedRowKeys, selectedRows) {
      this.selectedRows = selectedRows;
      this.$emit("onSelectChange", selectedRowKeys, selectedRows);
    },
    paginationChange(e) {
      const { current } = e;
      this.$emit("paginationChange", current);
    }
  }
};
</script>
<style lang="less" scoped>
@import "../styles/index.less";
.container {
  display: flex;
  justify-content: center;
  padding: 20px;
  min-height: 500px;

  .leftContainer {
    display: flex;
    float: left;
    min-width: 300px;
    border: 1px solid #e6f0fb;
    padding: 10px;
  }

  .rightContainer {
    display: flex;
    flex-direction: column;
    border: 1px solid #e6f0fb;
    padding: 10px;

    .top-right-container {
      display: flex;
      padding: 10px;

      .search {
        justify-content: flex-start;
      }

      .selectedConfirm {
        display: flex;
        flex: 1;
        justify-content: flex-end;
      }

      .search-button {
        margin-left: 5px;
      }
    }
    // flex: 1;
    // background: #f0ad4e;
  }
}
/deep/ .ant-tree li span.ant-tree-switcher {
  width: 16px;
  height: 16px;
  margin: 4px;
  // 修改树结构合起的icon
  &.ant-tree-switcher_close {
    background: url("//assets.2dfire.com/frontend/b415e20fc703838e5a028437366ff22a.png")
      no-repeat;
    background-size: contain;
    i {
      display: none;
    }
  }
  // 修改树结构展开的icon
  &.ant-tree-switcher_open {
    background: url("//assets.2dfire.com/frontend/568ca02f82eee05829d276881363c22a.png")
      no-repeat;
    background-size: contain;
    i {
      display: none;
    }
  }
}
</style>
