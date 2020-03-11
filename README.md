####  vue-element
基于Element的自定义的组件
##### 1. SelectTree
基于Element的el-select和el-tree实现的简单的下拉树组件,并实现搜索功能。
使用方法如下,
```vue
 <SelectTree 
          :defaultProps="{children:'children',label:'label',id:'id'}"
          :treeData="与Element tree格式相同法人数组"
          :placeholder="请选择数据"
          v-model="数组格式"
          :multiple="Boolean 多选或者单选 默认多选"
          @node-change="选择数据变化时回调"
          :optionExtendStyle="{'max-width':'300px'}"
          />
```

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
数据格式例子如下
```javascript
transferData:{
​         height:'300px',
​         tableMaxHeight:'250px',
​         tableMaxWidth:'',
​         tableSize:'mini',
​         overflow:'auto',
​         defaultSort:{prop: 'name', order: 'descending'},
​         overridingStyles:{'margin-left': '-87px','margin-right': '-7px'},
​         tableColumnKeyName:'id',
​         options:{
​          source:{
​            title:'待选择',
​            data:[],//数据
​            likeParam:undefined,
​            searchCallBack:this.handleSearch,
​            column:[{prop:'name','姓名'},{prop:'age',label:'年龄'}],
​          },
​          target:{
​            title:'已选择',
​            data:[],
​            column:[{prop:'name','姓名'},{prop:'age',label:'年龄'}]
​          }
​         }
​        

      }      
```

