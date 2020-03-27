<template>
    <div class="page">
      <span class="total-text"> 共 {{ pageInfo.total }} 条 </span>

      <select class="form-control" @change="handlePageSize"  v-model="pageInfo.pageSize">
        <option v-for="(sItem,index) in pageInfo.pageSizes" :value="sItem.value"   :key="index"  >
          {{  sItem.name | filerPageSizes }}
        </option>
      </select>
      <span :class="[currentPage === 1?'no-activite' : 'can-activite','operation'] " @click="handleCurrenPage('up')" >上一页 </span>
          <span v-show="startPage > 1 && pageInfo.pageNums >= 7" > ... </span>
          <span  :class="[currentPage===pageVal ? 'activited':'can-activite']"
                 v-for="pageVal in getShowPages()" :key="pageVal" @click="handleCurrenPage(pageVal)"> {{ pageVal }} </span>
          <span v-show="endPage < pageInfo.pageNums"> ... </span>
      <span :class="[currentPage === pageInfo.pageNums?'no-activite' : 'can-activite','operation'] " @click="handleCurrenPage('next')">下一页</span>
    </div>
</template>

<script>
    export default {
      props:{
        pageInfo:{
          type: Object,
        },
       },
      data(){
        return {
         currentPage:1,
         startPage:0,
         endPage:0
        }
      },
      watch:{ },
      filters:{
        filerPageSizes(item){
          if (Number.isInteger(item)) {
             return item +'条/页'
          }else {
            return item
          }
        }
      },
      created(){
            //   this.getPageNums()
      },
      updated(){
           this.getPageNums()
      },
      methods:{

        // 触发选择 条/页 事件
        handlePageSize(type){
           //  console.log('handlePageSize: ',this.pageInfo.pageSize)
           //   this.getPageNums()
            this.currentPage = 1
            this.$emit('handlePageSize', this.pageInfo.pageSize);
        },
        // 触发当前页码
        handleCurrenPage(val){
           // console.log('handleCurrenPage:',val)
           //上一页处理
           if (val === 'up') {
             if (this.currentPage === 1) return
             this.currentPage =   this.currentPage - 1
           }
          //下一页处理
           else  if (val === 'next') {
            if (this.currentPage === this.pageInfo.pageNums) return
             this.currentPage =   this.currentPage+ 1
          }  else {
             if ( this.currentPage === val) return
             this.currentPage = val
           }
          this.$emit('handleCurrenPage', this.currentPage);
        },
        // 计算共有多少页
        getPageNums(){
            let  nums = parseInt( this.pageInfo.total / this.pageInfo.pageSize)
            if (this.pageInfo.total % this.pageInfo.pageSize > 0){
              nums += 1
            }
            this.pageInfo.pageNums = nums
      
      //    console.log('getPageNums :', this.pageSize)

        },
        // 计算当前显示的页码
        getShowPages(){
           let showPages = []
           let start = this.currentPage > 3? this.currentPage - 3 : 1
            let end = this.pageInfo.pageNums > 7? this.currentPage + 3:this.pageInfo.pageNums
            if(this.pageInfo.pageNums <= 7) {
                 start = 1
                 end = this.pageInfo.pageNums
            } else {
                if (start === 1) end = 7
                if (end >  this.pageInfo.pageNums){
                  start = (start - (end - this.pageInfo.pageNums) ) <= 0? 1 :  (start - (end - this.pageInfo.pageNums) )
                  end = this.pageInfo.pageNums
                }
            }
            this.startPage =  start
            this.endPage = end
            for (start; start <= end; start++) {
              showPages.push(start)
            }
            return showPages
        }

      }

    }
</script>

<style scoped>
  .page {
    white-space: nowrap;
    padding: 2px 5px;
    color: #303133;
    font-weight: 700;
  }
  .total-text {
    margin-right: 10px;
    font-weight: 400;
    color: #606266;
    font-size: 1rem;
    line-height: 1.5;
  }
  .form-control{
    padding: .175rem .275rem;
    font-size: 1rem;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: .25rem;
    transition: border-color .15s ease-in-out,box-shadow .15s ease-in-out;
  }

  /* 选择的样式 */
 .activited {
   color: #409EFF;
   cursor: default;
 }
 .operation{
   white-space: nowrap;
   padding: 2px 5px;
   color: #303133;
   font-weight: 700;
 }

  /*可以选择*/
  .can-activite{
    cursor: pointer;
  }
  /*不可以选择*/
  .no-activite{
    color:#C0C4CC;
    cursor: not-allowed;
  }

</style>
