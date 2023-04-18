
<template>
  <div>
    <el-main>
      <!--表格，详情查看官网使用方法-->

        <template>
          <el-table
              :data="info.tableData.slice((info.currentPage-1)*info.pageSize,info.currentPage*info.pageSize)"
              border
              style="width: 100%">
            <el-table-column
                fixed
                prop="date"
                label="Avatar"
                width="300">
              <template slot-scope="{ row }">
                <el-image style="width: auto; height: 40px; border: none; cursor: pointer"
                          :src="row.image"></el-image>
              </template>
            </el-table-column>
            <el-table-column
                prop="firstName"
                label="firstName"
                width="500">
            </el-table-column>
            <el-table-column
                prop="lastName"
                label="lastName"
                width="400">
            </el-table-column>
            <el-table-column
                fixed="right"
                label="Operation"
                width="200">
              <template slot-scope="scope">
                <el-button @click="handleClick(scope.row)" type="text" size="small">查看详情</el-button>
                <el-button type="text" size="small">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
        </template>

      <template>
        <div class="block">
          <span class="demonstration"></span>
          <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :page-sizes="[10, 5, 15, 20]"
              :page-size="10"
              layout="total, sizes, prev, pager, next, jumper"
              :total="info.totalCount">
          </el-pagination>
        </div>
      </template>

<!--      &lt;!&ndash;分页工具条&ndash;&gt;-->
<!--      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"-->
<!--                     :background="background" :current-page="currentPage" :page-sizes="[10, 20, 30, 40]" :page-size="10"-->
<!--                     layout="total, sizes, prev, pager, next, jumper" :total="totalCount">-->
<!--      </el-pagination>-->

    </el-main>
<!--    <el-table :data="info.tableData.slice((info.currentPage-1)*info.pageSize,info.currentPage*info.pageSize)" stripe style="width: 100%">-->
<!--      <el-table-column prop="firstName" label="项目名称" width="150"></el-table-column>-->
<!--      <el-table-column prop="lastName" label="项目代码" width="150"></el-table-column>-->
<!--    </el-table>-->
<!--    <el-pagination-->
<!--        :current-page.sync="info.currentPage"-->
<!--        @current-change="handlePageChange"-->
<!--        :page-size="info.pageSize"-->
<!--        :page-sizes = "[2, 10, 50, 500]"-->
<!--        layout="total, prev, pager, next"-->
<!--        :total="info.dataCount">-->
<!--    </el-pagination>-->

  </div>
</template>

<script>
import axios from "axios";
export default {
  name:'MyList',
  // components: {ToPage},
  data() {
    return {
      info:{
        dialogTableVisible: false,
        dialogFormVisible: false, // 详情表单
        dialogVisible: false,   // 删除对话框
        formLabelWidth: '120px',
        pageSize: 10, // 每页显示10条
        totalCount: 50, // 总记录数初始化为100
        currentPage: 1, // 当前页码
        tableData: [
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'002',firstName: 'Tom',lastName: 'TIm'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'001',firstName: 'jack',lastName: 'jon'},
          {userid:'002',firstName: 'Tom',lastName: 'TIm'},
          {userid:'002',firstName: 'Tom',lastName: 'TIm'},
          {userid:'002',firstName: 'Tom',lastName: 'TIm'},
          // {userid:'003',firstName: 'quan',lastName: 'zhou'}
        ], // 接收返回的数据
        deleteName: "", // 删除的username名称
        form:[{ // 用来显示详情数据和更新数据
          Avatar: "",
          userName: "",
          passWord: "",
          firstName: "",
          lastName: "",
          telephone: "",
          email: ""
        }],
        staff: {
          image: ""
        }
    }
  }
  },

  methods:{
    // handlePageChange(pageNum) {
    //   this.searchItem.limit = this.pageSize;
    //   this.searchItem.page = pageNum;
    //   this.currentPageNum = this.searchItem.page;
    //   this.search(this.searchItem);
    // },
    // search(argumentObj) {
    //   let self = this;
    //   const BaseUrl = this.$util.getAjaxUrl('type_8')
    //   const url = BaseUrl + '/api/tProjectManagement/getTProjectManagementList'
    //   axios.get(url,argumentObj).then(function(response) {
    //     if (response.data.status === -1) {
    //       self.$message.error({
    //         content: response.data.msg
    //       })
    //     } else {
    //       self.tableData = response.data.data.list;
    //       self.dataCount = response.data.data.total;
    //       self.loadingList = false
    //     }
    //   }).catch(function(error) {
    //     console.log(error);
    //   });
    // },

    deleteByuserName() {
      this.info.dialogVisible = false;
      let that = this;
      console.log(that.deleteName);
      axios.delete("http://localhost:8080/admin/staff/" + that.deleteName)
          .then(function (resp) {
            if(resp.data.code === 1){
              that.info.tableData = resp.data.data;
              that.reload();
            }else{
              alert(resp.data.msg);
            }

            // that.tableData = resp.data;
            // that.reload();
          })
    },
    handleSizeChange(val) {
      this.info.pageSize=val;
      // console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {

      this.info.currentPage=val;
      // console.log(`当前页: ${val}`);
    },
    handleEdit(index, row) {
      this.info.dialogFormVisible = true
      this.info.form.Avatar = row.avatar
      this.info.form.firstName = row.firstname
      this.info.form.lastName = row.lastname
      this.info.form.userName = row.username
      this.info.form.passWord = row.password
      this.info.form.email = row.email
      this.info.form.telephone = row.telephone
    },
    bandDeleteName(row) {
      this.info.dialogVisible = true;
      this.info.deleteName = row.username;
    }

    // handleSet (index, row) {
    //     var params = {
    //         Avatar: this.form.Avatar,
    //         userName: this.form.userName,
    //         passWord: this.form.passWord,
    //         firstName: this.form.firstName,
    //         lastName: this.form.lastName,
    //         telephone: this.form.telephone,
    //         email: this.form.email
    //     }

    // }
  },

  mounted(){
    //从搜索框中获取信息
    // this.$bus.$on('updateListData',(dataObj)=>{
    //   this.info = {...this.info,...dataObj}
    // })
    this.info.totalCount=this.info.tableData.length
  },
  beforeDestroy() {
    // this.$bus.$off('updateListData')
  }

}
</script>

<style scoped>

</style>