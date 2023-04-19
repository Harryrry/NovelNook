
<template>
  <div>
    <!-- <el-header>
               导航栏
           </el-header> -->
    <el-main>
      <!--搜索表单-->
      <el-form :inline="true" :model="searchStaff.username">
        <el-form-item label="username">
          <el-input v-model="searchStaff.username" placeholder="Please input the username"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="selectByuserName(searchStaff.username)">Search</el-button>
          <el-button type="info" @click="clear">Clear</el-button>
        </el-form-item>
      </el-form>

      <br>
      </el-main>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name:'ToSearch',
  data(){
     return{
       searchStaff: { // 接收输入的username
      username: ""
    }}
  },
  methods:{
    selectByuserName(userName) {
      let that = this;
      axios.get("http://localhost:8080/admin/staff/" + userName).then(
          resp => {
        this.$bus.$emit('updateListData',{tableData:resp.data.data})
        if(resp.data.code === 1){
          that.tableData = resp.data.data;
        }else{
          alert(resp.data.msg);
        }
      })
    },
    clear() {
      this.searchStaff.username = ''
    },
  }
}
</script>

<style scoped>

</style>