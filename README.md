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
:table-columns="tableClumns"
:table-data-source="tableDataSource"
@onTreeSelect="onTreeSelect"
@handleSearch="handleSearch"
@handleSelected="handleSelected"
@onSelectChange="onSelectChange"
@paginationChang="paginationChange"
/>
```
