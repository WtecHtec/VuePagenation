<template>
<div>
  <el-button @click="setParam"> 修改参数</el-button>
   <PageCom  :page-info="pageInfo" @handlePageSize="handlePageSizeVal"  @handleCurrenPage="handleCurrenPageVal"></PageCom>
 <el-pagination
      :current-page.sync="currentPage2"
      :page-sizes="[100, 200, 300, 400,'所有']"
      :page-size="100"
      layout="sizes, prev, pager, next"
      :total="1000">
    </el-pagination>
 </div>
</template>
<script>
import  PageCom from './wtecthtec-pagenation'
export default {
  components:{
    PageCom
  },
  data () {
    return {
      pageInfo:{
          pageSizes:[
            {value: 10,name: 10},
            {value: 20,name: 20},
            {value: 30,name: 30},
          ],
        pageSize: 10,
        total:100,
        pageNums:10
      }
     
    }
  },
  created () {

  },
  methods: {
    setParam(){
      this.pageInfo.pageSize = 20
    },
    handlePageSizeVal(val){
      console.log(`当前选条/页 ${val}`)
    },
    handleCurrenPageVal(val){
       console.log(`当前页 ${val}`)
    }
  }
}
</script>
