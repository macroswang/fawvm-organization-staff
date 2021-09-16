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
        :table-size="'small'"
        :target-keys="targetKeys"
        :tree-data-source="treeDataSource"
        :table-left-columns="tableLeftColumns"
        :table-right-columns="tableRightColumns"
        :table-data-source="tableDataSource"
        @onTreeSelect="onTreeSelect"
        @handleSearch="handleSearch"
        @handleSelected="handleSelected"
        @onSelectChange="onSelectChange"
      />
 说明:
   使用了ant-design-vue中的 Transfer 穿梭框组件
   
 字段说明如下:
 tableLeftColumns: 中间部分,代表数据源
 tableRightColumns: 右边部分,代表所选的内容
 targetKeys:右边带入默认已选中的内容,
 treeDataSource:左边树的数据源
 
 使用特别说明:
   由于Transfer 穿梭框组件对数据源有特别的要求:
     tableDataSource数据源中必须包含title和key字段(详情见ant-design-vue关于Transfer的描述),示例如下:
     {
        title: "小何",
        key: "hexiao4@xxx.com",
        email: "hexiao4@xxx.com",
        deptName: "技术部",
        vwdutyName: "后端"
      }
```

![image-20210916170949466](/Users/macroswang/Library/Application Support/typora-user-images/image-20210916170949466.png)
