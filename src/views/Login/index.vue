<template>
  <div id="login">
    <div class="login-warp">
      <ul class="menu-tab">
        <li
          :class="{current:temp==index}"
          v-for="(item,index) in list"
          :key="item.id"
          @click="toggleMneu(index)"
        >{{ item.text }}</li>
      </ul>
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        class="login-form"
        size="small"
      >
        <el-form-item prop="username">
          <label>邮箱</label>
          <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item prop="password">
          <label>密码</label>
          <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item prop="code">
          <label>验证码</label>
          <el-row :gutter="10">
            <el-col :span="18">
              <el-input v-model.number="ruleForm.code"></el-input>
            </el-col>
            <el-col :span="5">
              <el-button type="success" round>验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>

        <el-form-item>
          <el-button type="danger" @click="submitForm('ruleForm')" class="block">提交</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  data() {
    /* 表达验证 */

    //验证用户名
    var validateUsername = (rule, value, callback) => {
      let reg = /\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*/;
      if (value === "") {
        callback(new Error("请输入用户名"));
      } else if (!reg.test(value)) {
        callback(new Error("请输入正确的邮箱格式"));
      } else {
        callback();
      }
    };
    // 验证密码
    var validatePassword = (rule, value, callback) => {
      let reg = /^\w+$/;
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (!reg.test(value)) {
        callback(new Error("请输入正确的密码格式"));
      } else if (!(value.length <= 15 && value.length >= 6)) {
        callback(new Error("请输入6到15位的密码"));
      } else {
        callback();
      }
    };
    //验证验证码
    var checkCode = (rule, value, callback) => {
      let reg = /^[a-z0-9]+$/i;
      if (value == "") {
        return callback(new Error("请输入验证码"));
      } else if (!reg.test(value)) {
        return callback(new Error("验证码格式不正确"));
      } else {
        callback();
      }
    };
    return {
      list: [{ text: "登录" }, { text: "注册" }],
      temp: 0, //默认选择登录

      /* 表单数据 */
      ruleForm: {
        username: "",
        password: "",
        code: ""
      },
      rules: {
        username: [{ validator: validateUsername, trigger: "blur" }],
        password: [{ validator: validatePassword, trigger: "blur" }],
        code: [{ validator: checkCode, trigger: "blur" }]
      }
    };
  },
  methods: {
    // 登录注册高亮
    toggleMneu(data) {
      this.temp = data;
    },
    // 表单提交方法
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;
}
.login-warp {
  width: 330px;
  margin: 0 auto;
  .menu-tab {
    text-align: center;
    padding: 0;
    margin: 0;
    li {
      padding: 0;
      margin: 0;
      list-style: none;
      display: inline-block;
      width: 60px;
      cursor: pointer;
      padding: 5px;
    }
    .current {
      background-color: #2e4157;
      border-radius: 2px;
    }
  }
}
.login-form {
  margin-top: 30px;
  .block {
    display: block;
    width: 100%;
  }
}
</style>
