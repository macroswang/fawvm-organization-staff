<template>
  <div class="container">
    <div class="leftContainer">
      <a-tree
        :show-icon="treeShowIcon"
        :show-line="treeShowLine"
        :tree-data="treeDataSource"
        @select="onTreeSelect"
        :default-expanded-keys="treeExpandedKeys"
      >
      </a-tree>
    </div>
    <div class="rightContainer">
      <a-transfer
        :data-source="wapperTableDataSource"
        :target-keys="selectedRowKeys"
        :operations="['选中', '移除']"
        :filter-option="
          (inputValue, item) => item.title.indexOf(inputValue) !== -1
        "
        :show-select-all="false"
        @change="onChange"
      >
        <template
          slot="children"
          slot-scope="{
            props: { direction, filteredItems, selectedKeys },
            on: { itemSelectAll, itemSelect }
          }"
        >
          <a-table
            :row-key="(record, index) => record.key"
            :row-selection="
              getRowSelection({
                selectedKeys,
                itemSelectAll,
                itemSelect
              })
            "
            :columns="
              direction === 'left' ? tableLeftColumns : tableRightColumns
            "
            :data-source="filteredItems"
            :size="tableSize"
            @change="paginationChange"
          />
        </template>
      </a-transfer>
      <div class="selectedConfirm">
        <a-button class="search-button" @click="handleReset">重置</a-button>
        <a-button class="search-button" type="primary" @click="handleSelected"
          >确定选择</a-button
        >
      </div>
    </div>
  </div>
</template>
<script>
import difference from "lodash/difference";

export default {
  data() {
    return {
      tempRowData: [],
      selectedRow: [],
      mapSelectedData: null,
      wapperTableDataSource: [],
      selectedRowKeys: []
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
    treeCurrentKey: {
      type: String,
      default: ""
    },
    treeDataSource: {
      type: Array,
      default: () => []
    },
    treeExpandedKeys: {
      type: Array,
      default: () => []
    },
    tableSize: {
      type: String,
      default: ""
    },
    tableLeftColumns: {
      type: Array,
      default: () => []
    },
    tableRightColumns: {
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
    targetKeys: {
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
      this.wapperTableDataSource =
        val &&
        val.map(item => {
          return Object.assign(item, { pid: this.treeCurrentKey });
        });
    },
    treeCurrentKey(val) {
      this.treeCurrentKey = val;
    },
    targetKeys(val) {
      val &&
        val.map(item => {
          if (item.pid === this.treeCurrentKey) {
            this.selectedRowKeys.push(item.key);
          }
        });
    }
  },
  created() {
    this.tempRowData = [...this.targetKeys];
    //默认展开第一级树
    if (this.treeExpandedKeys && this.treeExpandedKeys.length === 0) {
      this.treeExpandedKeys.push(this.treeDataSource[0].key);
    }
  },
  methods: {
    handleSearch() {
      this.$emit("handleSearch", this.partNo);
    },
    handleReset() {
      this.selectedRow = [];
      this.tempRowData = [];
      this.selectedRowKeys = [];
      this.$emit("handleReset");
    },
    handleSelected() {
      this.selectedRow = [];
      this.selectedRowKeys.map(val => {
        this.tempRowData.some(item => {
          if (val === item.key && !this.selectedRow.includes(item)) {
            return this.selectedRow.push(item);
          }
        });
      });
      if (this.selectedRow.length === 0) {
        this.selectedRow = [...this.tempRowData];
      }
      this.$emit("handleSelected", this.selectedRow);
    },
    onTreeSelect(selectedKeys, info) {
      this.$emit("onTreeSelect", selectedKeys, info);
    },
    paginationChange(e) {
      const { current } = e;
      this.$emit("paginationChange", current);
    },
    onChange(nextTargetKeys) {
      this.selectedRowKeys = nextTargetKeys;
      nextTargetKeys.map(val => {
        this.wapperTableDataSource.some(item => {
          if (val === item.key && !this.tempRowData.includes(item)) {
            return this.tempRowData.push(item);
          }
        });
      });
    },
    getRowSelection({ selectedKeys, itemSelectAll, itemSelect }) {
      return {
        // getCheckboxProps: item => ({
        //   props: { disabled: disabled || item.disabled }
        // }),
        onSelectAll(selected, selectedRows) {
          const treeSelectedKeys = selectedRows.map(({ key }) => key);
          const diffKeys = selected
            ? difference(treeSelectedKeys, selectedKeys)
            : difference(selectedKeys, treeSelectedKeys);
          itemSelectAll(diffKeys, selected);
        },
        onSelect({ key }, selected) {
          itemSelect(key, selected);
        },
        selectedRowKeys: selectedKeys
      };
    }
  }
};
</script>
<style lang="less" scoped>
@import "../styles/index.less";
.container {
  display: flex;
  justify-content: center;

  .leftContainer {
    display: flex;
    float: left;
    min-width: 280px;
    max-height: 600px;
    overflow-x: hidden;
    border: 1px solid #e6f0fb;
    padding: 10px;
  }
  .rightContainer {
    display: flex;
    flex: 1;
    flex-direction: column;
    max-height: 600px;
    overflow-x: hidden;
    border: 1px solid #e6f0fb;
    padding: 5px;
  }

  .selectedConfirm {
    display: flex;
    flex: 1;
    padding-top: 10px;
    padding-bottom: 10px;
    justify-content: flex-end;
  }

  .search-button {
    margin-left: 5px;
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
