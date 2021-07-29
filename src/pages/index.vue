<template>
  <div class="coontainer">
    <div class="main">
      <div class="list" v-for="(item,index) in popList" :key="index">
        <img :src="item.identityCardUrl" alt="">
        <div class="content">
          <div class="idcard">{{item.id}}</div>
          <div class="name">{{item.userName}}</div>
          <div class="gender">{{item.sex == 'male' ? '男' : '女'}}</div>
          <div class="address">{{item.address}}</div>
        </div>      
      </div>
    </div>    
    <!-- 
      currentPage: 当前页码，就是当前页面是第几页
      total: 总数量，比如搜索出的结果一共有多少条
      pageSize: 每一页显示的数据条数
      showPageNo: 连续页码数
      currentChange：页码发生变化时触发的事件
     -->    
    <Pageination
      :currentPage="curPage"
      :total="total"
      :pageSize="pageSize"
      :showPageNo="showPageNo"
      @currentChange="pageChange"   
    ></Pageination>    
  </div>
</template>

<script>
import Pageination from '@/components/Pageination'
export default {
  name: "page",
  components:{
    Pageination
  },
  data(){
    return {
      popList:{}, 
      curPage:1,//当前页
      total:10,//总共页数
      pageSize:3,//每页记录数 
      showPageNo:3, // 连续页码数
    }
  }, 
  mounted(){
    let requrl = 'http://yapi.luckly-mjw.cn/mock/50/test/users?pageIndex=1';
    this.$axios.get(requrl).then(res => {
      this.popList = res.data.data.list;
      let pages = res.data.data.page;
      console.log(res.data)
      // console.log(this.popList)
      this.curPage = Number(pages.pageIndex);
      // this.pageSize = Number(pages.pageSize);
      this.total = Number(pages.pageSum);
    }).catch(
        function (error) {
            // 请求失败处理
        console.log('请求失败！'+error)
    })
  },   
  methods:{
    pageChange(e){
      console.log(e)
      this.curPage = e;
    }
  }
}
</script>

<style lang="scss" scoped>
.main{
  width:30%;
  margin: 0 auto;
  padding-bottom: 40px;
  .list{
    display: flex;
    margin-bottom: 20px;
    .content{
      text-align: left;
      padding-left: 10px;
      line-height: 1.5;
    }
  }
}
</style>