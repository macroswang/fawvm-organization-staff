# fawvm-organization-staff

> 组织机构与人员管理前端 UI

## Build Setup

```bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# use
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
 说明:
   使用了ant-design-vue中的 Transfer 穿梭框组件

 字段说明如下:
 tableLeftColumns: 中间部分,代表数据源
 tableRightColumns: 右边部分,代表所选的内容
 targetKeys:右边带入默认已选中的内容,
 treeDataSource:左边树的数据源

 v1.0.7版本:
 新加入tree-current-key字段:此字段的作用是传入树的当前选中的节点KEY
 v1.0.8版本:
 修改BUG及数据回填优化

 使用特别说明:
   由于Transfer 穿梭框组件对数据源有特别的要求:
     tableDataSource数据源中必须包含title和key字段(详情见ant-design-vue关于Transfer的描述),示例如下:
     {
        title: "小何",
        key: "hexiao4@xxx.com",
        email: "hexiao4@xxx.com",
        pid:"188888",
        deptName: "技术部",
        vwdutyName: "后端"
      }
使用方法:如代码所示:
 methods: {
    handleReset() {
      this.receiver = "";
      this.targetKeys = [];
      this.selectedRows = [];
    },
    handleSelected(selectedRows) {
      //do something
      this.organizationVisible = false;
      this.targetKeys = [];
    },
    onSelectChange(selectedRow, selectedRowKey) {
      console.log("当前点击选择的内容:", selectedRow, selectedRowKey);
    },
    // 菜单选择
    onTreeSelect(selectedKeys, info) {
      // 设置表格中的数据源
      this.treeCurrentKey = selectedKeys[0];
      if (selectedKeys[0] === "0-0-0-1") {
        this.tableDataSource = tableDataSource1;
      } else if (selectedKeys[0] === "0-0-0-2") {
        this.tableDataSource = tableDataSource2;
      }
      console.log(info.node.title);
    },
    //弹出对话框时注意初始化变量
    handleVisible() {
      this.organizationVisible = true;
      this.tableDataSource = [];
      this.treeCurrentKey = "";
      this.targetKeys = [...this.selectedRows]; //如有已选数据,可回填
    }
  }
};
```
