<template>
  <el-select :value="valueTitleArray" :filter-method="filterMethod" 
    filterable
    multiple
    collapse-tags
    :disabled="disabled"
    :clearable="clearable"
    :placeholder="placeholder"
    @remove-tag="clearHandle"
     @clear="clearHandle">
        <el-option :value="option_id"  :label="option_label"   :style="[{'height': 'auto','width': 'auto',
        'padding': '0',
        'overflow-x': 'scroll'},optionExtendStyle]">
      <div class="treeDiv">
           <el-tree  
            id="tree-option"
            ref="selectTree"
            :style="treeStyle"
            :accordion="accordion"
            :data="treeData"
            :props="defaultProps"
            :show-checkbox="showCheckBox"
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
    //select组件 disabled
    disabled:{ type:Boolean, default: false },
    placeholder:{type:String, default: "请选择"},
    // 选项列表数据(树形结构的对象数组)
    treeData:{ type: Array, default: [] },
    // 可清空选项
    clearable:{ type:Boolean, default: true },
    // 自动收起
    accordion:{ type:Boolean, default: false },
    // tree 选择模式checkbox
    showCheckBox:{type:Boolean, default: true},
    //可配置tree的Style
    treeStyle:{
        type:Object,
        default(){
          return {}
        }
    },
    //监听v-model
    value:{type:Array},
    //options 扩展配置
    optionExtendStyle:{
      type:Object,
      default(){
        return {
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
    this.initCheckNode();
  },

  methods: {
    initCheckNode(){
      if (this.value!=null && this.value.length>0){
        this.checkedNodes = this.value
      }
    },
    handleNodeClick(item, node, self){
      if(this.showCheckBox){
        //多选
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
      }else{
        //单选
        this.valueTitleArray = []
        this.checkedNodes = []
        let label = node.label
        if(label.length>8){
          label = label.slice(0,5)+"..."
        }
        this.valueTitleArray.push(label)
        this.checkedNodes.push(node)
      }
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
    },
    // 清除选中
    clearHandle(){
      if(this.valueTitleArray){
        this.valueTitleArray.pop()
        this.checkedNodes.pop()
      }
      this.$refs.selectTree.setCheckedNodes(this.checkedNodes)

    },
  
    filterMethod(val){
      this.$refs.selectTree.filter(val)
    },

    filterNode(value, data) {
        if (!value) return true;
        return data.label.indexOf(value) !== -1;
    }
  },
  watch: {
   treeData: {
      deep: true,  // 深度监听
      handler(newVal,oldVal) {
        if(newVal.length>0&& this.defaultExpandedKey.length==0){
          this.defaultExpandedKey=[newVal[0].id]
        }
      }
    },
    value(newVal) {
      this.checkedNodes = newVal
    },

    checkedNodes(newVal){
       this.$emit('input',this.checkedNodes)
    }

  },
  computed:{

  }
  
}
</script>

<style scoped>

  .el-select-dropdown__item.selected{
    font-weight: normal;
  }
 ul li >>>.el-tree .el-tree-node__content{
    height:auto;
    padding: 0 20px;
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