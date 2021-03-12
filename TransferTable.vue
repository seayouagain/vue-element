<template>
<div>
<el-row >
  <el-col :span="10">
          <div class="search-inp-container">
            <el-input  :placeholder="$t('m.group.search_key')"
                      v-model="options.source.likeParam"
                      class="input-with-select"
                      size="mini"
                       clearable
                       @clear="handleSearch"
                      @keyup.enter.native="handleSearch">
              <el-button slot="append"
                         icon="el-icon-search"
                         size="mini"
                         v-on:click="handleSearch"></el-button>
            </el-input>
          </div>
  </el-col>
</el-row>
<el-row :gutter="20"  :style="[{height:height+'px',width:width+'px'}, overridingStyles]">
    <el-col :span="11" v-bind:style="{'max-width':tableMaxWidth, 'overflow':overflow}">
      <div style=" height: 28px;padding-left: 10px;color: #909399;">{{this.options.source.title}}</div>
        <el-table
            ref="sourceTable"
            :key="tableKey"
            :data="options.source.data"
            :size="tableSize"
            :default-sort="defaultSort"
            :height="tableHeight"
            :max-height="tableMaxHeight"
            @selection-change="handleSourceChange"
        >
        <el-table-column
          type="selection"
          align="center"
          :disabled = "chechBoxDisabled"
          width="55">
      </el-table-column>
        <template v-for="item in options.source.column">
          <el-table-column :key="item.prop"
                         show-overflow-tooltip
                         align="center"
                         :prop="item.prop"
                         :label="item.label">
          </el-table-column>
        </template>
        </el-table>
    </el-col>
    <el-col :span="2" :style="{'padding-top':(height-100)/2+20+'px'}">
        <el-button
            @click="toRight"
            type="primary"
            :title="$t('m.common.tarnsfer_add')"
            :disabled="!selectedLeft.length"
            icon="el-icon-arrow-right"
            size="mini"

        ></el-button>
        <el-button
            @click="toLeft"
            type="primary"
            :title="$t('m.common.tarnsfer_remove')"
            :disabled="!selectedRight.length"
            icon="el-icon-arrow-left"
            size="mini"

            style="margin-left: 0;margin-top: 10px;"
        ></el-button>
    </el-col>
    <el-col :span="11"  v-bind:style="{'max-width':tableMaxWidth,'overflow':overflow}">
      <div style=" height: 28px;padding-left: 10px;color: #909399;">{{this.options.target.title}}</div>
        <el-table
            ref="targetTable"
            :key="tableKey"
            :size="tableSize"
            :default-sort="defaultSort"
            :data="options.target.data"
            :height="tableHeight"
            :max-height="tableMaxHeight"
            @selection-change="handleTargetChange"
          >
          <el-table-column
          type="selection"
          align="center"
          :disabled = "chechBoxDisabled"
          width="55">
         </el-table-column>
          <template v-for="item in options.target.column">
            <el-table-column :key="item.prop"
                          align="center"
                           show-overflow-tooltip
                          :prop="item.prop"
                          :label="item.label">
            </el-table-column>
        </template>
        </el-table>
    </el-col>
</el-row>
</div>
</template>

<script>

export default {
  props: {
    options: {
      type: Object,
      required: true
    },
    height:{
      type:String,
      default:'300'
    },
    width:{
      type:String,
      default:''
    },
    tableHeight:{
      type:String
    },
    tableMaxHeight:{
      type:String,
      default:'255px'
    },
    overflow:{
      type:String,
      default:'auto'
    },
    tableSize:{
      type:String,
      default:"mini"
    },
    tableMaxWidth:{
      type:String,
      default:''
    },
    defaultSort:{
      type:Object,
      required:true
    },
    chechBoxDisabled:{
      type:Boolean,
      default:false
    },
    overridingStyles:{
      type:Object
    },
    //行key
    tableColumnKeyName:{
      type:String,
      required:true,
    }
  },
  data() {
    return {
      tableKey:0,
      source:{
        likeParam:undefined,
        data:[],//数据
        searchCallBack:function(){},
        column:[{prop:'',label:''}],
      },
      target:{
        data:[],
        column:[{prop:'',label:''}]
      },
      //选中的左侧的数据
      selectedLeft:[],
      //选中的右侧的数据
      selectedRight:[]

    }
  },
  computed: {
    leftData() {
        return this.options.source.data
    }
  },
  watch: {
    leftData(newV,oldV) {
      //去掉右边已经存在的数据
      let key = this.tableColumnKeyName
       if(this.options.target.data.length>0){
         let target = this.options.target.data
         target.forEach(element=>{
           for(let i=0;i<newV.length;i++){
             if(element[key]==newV[i][key]){
               newV.splice(i,1)
               break
             }
           }
         })
       }
       this.options.source.data = newV

    }
  },

  mounted() {

  },
  beforeDestroy() {

  },

 destroyed() {


  },
  methods: {
    /**
     * 数据移动到右侧
     */
    toRight(){
    	let _this = this
      let source = this.options.source.data
      let target = this.options.target.data
      let columnKey = this.tableColumnKeyName
	    _this.selectedLeft.forEach(element=>{
         //删除源数据
        for(var i =0;i<source.length;i++){
          if(source[i][columnKey] == element[columnKey]){
            source.splice(i,1)
            break
          }
        }
        //添加目标数据
        target.push(element)
      })
      //清除数组
	    _this.selectedLeft = []
	    _this.$refs['sourceTable'].clearSelection()
    },

    toLeft(){
      let source = this.options.target.data
      let target = this.options.source.data
      let columnKey = this.tableColumnKeyName
      this.selectedRight.forEach(element=>{
         //删除数据
        for(var i =0;i<source.length;i++){
          if(source[i][columnKey] == element[columnKey]){
            source.splice(i,1)
            break
          }
        }
        //添加目标数据
        target.push(element)
      })
      //清除数组
      this.selectedRight = []
      this.$refs.targetTable.clearSelection();
    },

    //左侧数据变更
    handleSourceChange(val){
      this.selectedLeft = val

    },
    handleTargetChange(val){
      this.selectedRight = val
    },
    /**
     * 执行搜索
     */
    handleSearch(){
      this.options.source.searchCallBack()

    }

  }
}
</script>
