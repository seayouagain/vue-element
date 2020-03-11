####  vue-element
基于Element的自定义的组件
##### 1. SelectTree
基于Element的el-select和el-tree实现的简单的下拉树组件,并实现搜索功能。
使用方法如下,
```vue
 <SelectTree 
          :defaultProps="{children:'children',label:'label',id:'id'}"
          :treeData="与Element tree要求格式相同的数组"
          :placeholder="请选择"
          v-model="result[]"
          :optionExtendStyle="{'max-width':'300px'}"
         />
```
@getCheckedNodes 回调方法当选择发生变化时会触发

##### 2.TransferTable
简单实现的可穿梭的表格.
使用方法如下
```vue
  <transfer-table 
                :options="transferData.options" 
                :defaultSort="transferData.defaultSort"
                :overridingStyles="transferData.overridingStyles"
                :tableColumnKeyName="transferData.tableColumnKeyName"
                tableHeight="280px"></transfer-table>
```


