<template>
  <el-select  v-model="selectTitle" :filter-method="filterMethod" 
    filterable
    :multiple="multiple"
    :collapse-tags="multiple"
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
            :show-checkbox="multiple"
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
    //placeholder
    placeholder:{type:String, default: "请选择"},
    // 选项列表数据(树形结构的对象数组)
    treeData:{ type: Array, default: [] },
    // 可清空选项
    clearable:{ type:Boolean, default: true },
    // 自动收起
    accordion:{ type:Boolean, default: false },
    // tree 是否为多选模式
    multiple:{type:Boolean, default: true},
    //可配置tree的Style
    treeStyle:{
        type:Object,
        default(){
          return {}
        }
    },
    //监听v-model
    value:{type:Array,default:[]},
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
      checkedNodes:this.value,
      defaultExpandedKey:[],
      option_id:"",
      option_label:""
    }
  },
  beforeMount(){
  },
  beforeDestroy(){

  },
  destroyed(){
  },
  mounted(){
  },

  methods: {  
    handleNodeClick(item, node, self){
      if(this.showCheckBox){
        //多选
        this.checkedNodes = []
        let nodes = this.$refs.selectTree.getCheckedNodes();
        nodes.forEach(element=>{
          this.checkedNodes.push(element)
        })
      }else{
        //单选
        this.checkedNodes = []
        this.checkedNodes.push(item)
      }
      this.eventChange()
    },

    checkChange(item, node, self) {
      this.valueTitleArray = []
      this.checkedNodes = []
      let nodes = this.$refs.selectTree.getCheckedNodes();
      nodes.forEach(element=>{
        this.checkedNodes.push(element)
      })
      this.eventChange()
    },
    // 清除选中
    clearHandle(){
      if(this.checkedNodes&&this.checkedNodes.length>0){
        this.checkedNodes.pop()
      }
      this.$refs.selectTree.setCheckedNodes(this.checkedNodes)
      this.eventChange()
    },
  
    filterMethod(val){
      this.$refs.selectTree.filter(val)
    },

    filterNode(value, data) {
        if (!value) return true;
        return data.label.indexOf(value) !== -1;
    },
    
   eventChange(){
      this.$emit('input',this.checkedNodes)
      //node-change事件
      this.$emit('node-change',this.checkedNodes)
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
    value(newVal,oldVal) {
      this.checkedNodes = newVal
    }
  },
  computed:{
    selectTitle:{
      // getter
      get: function () {
        let result = undefined
        if(this.checkedNodes&&this.checkedNodes.length>0){
          result = []
          if(this.multiple){
            this.checkedNodes.forEach(element=>{
              let label = element.label
              if(label.length>8){
                label = label.slice(0,5)+"..."
              }
              result.push(label)
            })
          }else{
              let label = this.checkedNodes[0].label
              if(label.length>8){
                label = label.slice(0,5)+"..."
              }
              result = label
          }
        }else{
          result = undefined
        }
        
        return result
      },
      // setter
      set: function (newValue) {
      }
     
    }
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