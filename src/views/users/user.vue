<template>
    <div class="user-manage">
<div class="breadcrumb">
<el-breadcrumb separator-class="el-icon-arrow-right">
  <el-breadcrumb-item :to="{ path: '/layout/index' }">首页</el-breadcrumb-item>
  <el-breadcrumb-item>用户管理</el-breadcrumb-item>
</el-breadcrumb>
</div>
 <el-table  :data="tableData">
    <el-table-column 
        prop="nickname"
        label="姓名"
        width="130">
    </el-table-column>
      <el-table-column
        prop="createTime"
        label="日期"
        width="130">
    </el-table-column>
      <el-table-column
        prop="desc"
        label="个性签名"
        width="400">
    </el-table-column>
      <el-table-column
        prop="desc"
        label="用户头像"
        width="100">
        <template slot-scope="scope" >
            <img :src="scope.row.avatar" class="avatar">
        </template>
    </el-table-column>
    <el-table-column label="操作">
        <template slot-scope="scope">
            <el-button size="small" type="primary" @click="handleDetails()">查看详细</el-button>
            <el-button size="small" type="danger" @click="handleDelete(scope.row._id)">删除</el-button>
        </template>
    </el-table-column>
</el-table>
    </div>
</template>
<script>
export default {
    data(){
        return{
tableData:[]
        }
    },
    methods:{
getData(){
    this.$axios.get('/user').then(res=>{
        if(res.code==200){
            this.tableData=res.data
        }
    })
},
  handleDetails(){
        this.$router.push('/layout/userDetails')
    },
    handleDelete(id){
this.$confirm('此操作将永久删除该管理员，是否继续？','警告',{
confirmButtonText:'确定',
cancelButtonText:'取消',
type:'warning'
}).then(()=>{
    this.$axios.post('/user/delete',{userIds:[id]}).then(res=>{
     
            this.$message.success(res.msg)
    
    })
    // this.$message({
    //     type:'success',
    //     message:'删除成功！'
    // });
}).catch(()=>{
    this.$message({
        type:'info',
        message:'已被删除'
    })
})
    },
    },
  
    created(){
        this.getData()
    }
}
</script>

<style acoped lang="scss">
.user-manage{
    .avatar{
      width:60px;
      height:60px;  
    }
}
</style>
