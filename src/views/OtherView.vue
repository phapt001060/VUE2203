<template>
  <div>
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="ruleForm.username"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input v-model="ruleForm.password"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ruleForm: {
        username: '',
        password: '',
      },
      rules: {
        username: [
          { required: true, message: '请输入活动名称', trigger: 'blur' },
          { min: 3, max: 15, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入活动名称', trigger: 'blur' },
          { min: 4, max: 15, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          //alert('submit!');
          let url = 'http://localhost:8080/login';
          console.log('url >>>'+url);
          let data={
            'username' : this.ruleForm.username,
            'password' : this.ruleForm.password
          };
          console.log('data >>>');
          console.log(data);
          this.axios.post(url,data).then((response) => {
            console.log(response.data);
            if(response.data==1){
              this.$message({
                message: '登录成功!',
                type: 'success'
              });
            }else if(response.data==2){
              this.$message.error('账号或密码错误');
            }else if(response.data==3){
              this.$message.error('用户名不存在');
            }
          }).catch(function (error){
             console.log('响应结果为失败');
          })
        } else {
          alert("请输入账号密码")
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
}
</script>

