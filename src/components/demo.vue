<template>
<div id="inserUser">
  <h3>姓名 <el-input
    placeholder="请输入内容"
    v-model="name"
    clearable
    style="width:300px"
    maxlength=50>
  </el-input></h3>
  <h3>年龄 <el-input
    placeholder="请输入内容"
    v-model="age"
    clearable
    style="width:300px"
    type="number" minlength=1 maxlength=3>
  </el-input></h3>
  <el-button  type="primary" @click="addUser">新增用户</el-button>
  <el-table :data="userList" style="width: 100%">
    <el-table-column prop="id" label="ID" width="180"> </el-table-column>
    <el-table-column prop="name" label="姓名" width="180">
      <template slot-scope="scope">
        <el-input placeholder="姓名" v-model="scope.row.name" size="mini" type="input" maxlength=50></el-input>
      </template>
    </el-table-column>
    <el-table-column prop="age" label="年龄" width="120">
      <template slot-scope="scope">
        <el-input placeholder="年龄" v-model="scope.row.age" size="mini" type="number" minlength=1 maxlength=3></el-input>
      </template>
    </el-table-column>
    <el-table-column label="操作" width="280">
      <template slot-scope="scope">
      <el-button type="warning" @click="updateUser(scope.row)" size="small">修改用户</el-button>
      <el-button type="danger" @click="deleteUser(scope.row.id)" size="small">删除用户</el-button></template>
    </el-table-column>
  </el-table>
</div>
</template>

<script>
export default {
  data () {
    return {
      name: 'name',
      age: '12',
      userList: {}
    }
  },
  methods: {
    addUser () {
      this.$ajax.get('http://localhost:8762/insertData?name=' + this.name + '&age=' + this.age)
        .then((response) => {
          this.$message({
            message: response.data.error,
            type: 'success'
          })
          this.name = '';
          this.age = '';
          this.$ajax.get('http://localhost:8762/getData')
            .then((response) => {
              if(response.data.state=="1"){
                this.userList = response.data.data;
              }
            }).catch((response) => {
              this.$message({
                message: '系统错误',
                type: 'error'
            })
          })
        }).catch((response) => {
          this.$message({
            message: response.data.error,
            type: 'error'
          })
      })
    },
    deleteUser ($index){
      this.$ajax.get('http://localhost:8762/removeDataByID?id='+$index)
        .then((response) => {
          if(response.data.state=="1"){
            this.$message({
              message: '删除成功',
              type: 'success'
            })
            this.$ajax.get('http://localhost:8762/getData')
              .then((response) => {
                if(response.data.state=="1"){
                  this.userList = response.data.data;
                }
              }).catch((response) => {
              this.$message({
                message: '系统错误',
                type: 'error'
              })
            })
          }else{
            this.$message({
              message: '系统错误',
              type: 'error'
            })
          }
        }).catch((response) => {
          this.$message({
            message: '系统错误',
            type: 'error'
          })
      })
    },
    updateUser(data){
      this.$ajax.get('http://localhost:8762/updateDataByID?id='+data.id+"&name="+data.name+"&age="+data.age)
        .then((response) => {
          if(response.data.state=="1"){
            this.$message({
              message: '修改成功',
              type: 'success'
            })
            this.$ajax.get('http://localhost:8762/getData')
              .then((response) => {
                if(response.data.state=="1"){
                  this.userList = response.data.data;
                }
              }).catch((response) => {
              this.$message({
                message: '系统错误',
                type: 'error'
              })
            })
          }else{
            this.$message({
              message: '系统错误',
              type: 'error'
            })
          }
        }).catch((response) => {
        this.$message({
          message: '系统错误',
          type: 'error'
        })
      })
    }

  }

}

</script>
