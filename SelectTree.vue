<template>
  <el-select :value="valueTitleArray" :filter-method="filterMethod" 
    filterable
    multiple
    collapse-tags
   
    :clearable="clearable"
    :placeholder="placeholder"
    @remove-tag="clearHandle"
     @clear="clearHandle">
    <el-option :value="option_id" :label="option_label"   :style="[{'height': 'auto','width': 'auto',
        'padding': '0',
        'overflow': 'auto'},optionExtendStyle]">
      <div class="treeDiv">
           <el-tree  
            id="tree-option"
            ref="selectTree"
            :style="treeStyle"
            :accordion="accordion"
            :data="treeData"
            :props="defaultProps"
            show-checkbox
            node-key="id" 
            :check-strictly="true"
            :filter-node-method="filterNode"
            :default-expanded-keys="defaultExpandedKey"
            @check-change="checkChange"
            @node-click="handleNodeClick">
          </el-tree>   
      </div>
          
    </el-option>
  </el-select>
</template>

<script>
export default {
  name: "el-tree-select",
  props:{
    // 配置项
    defaultProps:{
      type: Object,
      default: {
          label: 'label',         // 显示名称
          children: 'children'    // 子级字段名
        }
    },
    placeholder:{type:String, default: "请选择"},
    // 选项列表数据(树形结构的对象数组)
    treeData:{ type: Array, default: [] },
    // 可清空选项
    clearable:{ type:Boolean, default: true },
    // 自动收起
    accordion:{ type:Boolean, default: false },
    //可配置tree的Style
    treeStyle:{
        type:Object,
        default(){
          return {}
        }
    },
    
    //options 扩展配置
    optionExtendStyle:{
      type:Object,
      default(){
        return {
          'max-height':'200px' ,
          'max-width': '250px'
        }
      }
    }  
  },
  data() {
    return {
      checkedNodes:[],
      valueTitleArray:[],
      defaultExpandedKey:[],
      option_id:"",
      option_label:""
    }
  },
  mounted(){
  },

  methods: {
  
    handleNodeClick(node){
      this.valueTitleArray = []
      this.checkedNodes = []
      let nodes = this.$refs.selectTree.getCheckedNodes();
      nodes.forEach(element=>{
         let label = element.label
        if(label.length>8){
          label = label.slice(0,5)+"..."
        }
        this.valueTitleArray.push(label)
        this.checkedNodes.push(element)
      })
      this.checkedNodesChange()
    },

    checkChange(item, node, self) {
      this.valueTitleArray = []
      this.checkedNodes = []
      let nodes = this.$refs.selectTree.getCheckedNodes();
      nodes.forEach(element=>{
        let label = element.label
        if(label.length>8){
          label = label.slice(0,5)+"..."
        }
        this.valueTitleArray.push(label)
        this.checkedNodes.push(element)
      })
      this.checkedNodesChange()
    },

    // 清除选中
    clearHandle(){
      if(this.valueTitleArray){
        this.valueTitleArray.pop()
        this.checkedNodes.pop()
      }
      this.$refs.selectTree.setCheckedNodes(this.checkedNodes)
      this.checkedNodesChange()

    },
  
    filterMethod(val){
      this.$refs.selectTree.filter(val)
    },

    filterNode(value, data) {
        if (!value) return true;
        return data.label.indexOf(value) !== -1;
    },

    checkedNodesChange(){
      this.$emit('getCheckedNodes',this.checkedNodes)
    }
  },
  watch: {
   treeData: {
      deep: true,  // 深度监听
      handler(newVal,oldVal) {
        if(newVal.length>0){
          this.defaultExpandedKey=[newVal[0].id]
        }
      }
    }
  },
  computed:{

  }
  
}
</script>

<style scoped>
   /* .el-scrollbar .el-scrollbar__view .el-select-dropdown__item{
    height: auto;  
    width: auto;
    max-height: 150px;
    max-width: 200px; 
    padding: 0;
    overflow: auto; 
  
  } */

  .el-select-dropdown__item.selected{
    font-weight: normal;
  }
 ul li >>>.el-tree .el-tree-node__content{
    height:auto;
    padding: 0 20px;
  } 
 /* .el-tree-node__label{
    font-weight: normal;
  } */
  .el-tree >>>.is-current .el-tree-node__label{
    color: #409EFF;
    font-weight: 700;
  }
  .el-tree >>>.is-current .el-tree-node__children .el-tree-node__label{
    color:#606266;
    font-weight: normal;
  } 
 .treeDiv{
    /* overflow-x: auto; */
    /* overflow-y: auto; */
    height: 100%;
    width: 100%;
    background-color: #ffffff;
   }
.el-tree{
  min-width: 100%;  
  font-size: 14px;  
  display: inline-block !important;  
}

</style>