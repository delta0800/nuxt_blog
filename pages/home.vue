
<template>
  <div>
    <el-container>
      <el-header>
        <el-menu
          mode="horizontal"
          router="true"
          @select="handleSelect"
          active-text-color="#0f87ff"
        >
          <el-menu-item index="/home/article">
            首页
          </el-menu-item>
          <el-menu-item index="/home/about">
            关于
          </el-menu-item>
          <div class="right">
            <el-button type="text"
              ><i class="el-icon-tickets"></i>写文章</el-button
            >
            <el-button type="text" @click="loginDialogVisible = true"
              >登录·注册</el-button
            >
          </div>
        </el-menu>
      </el-header>
      <el-main>
        <el-card class="box-card">
          <!-- {{ $store.state.counter }} -->
          <!-- {{ isLogin }} -->
          <router-view></router-view>
        </el-card>
      </el-main>
    </el-container>
    <el-dialog
      title="用户登录"
      :visible.sync="loginDialogVisible"
      width="50%"
      @close="loginDialogClosed"
    >
      <!-- 内容主体区 -->
      <el-form
        :model="loginForm"
        status-icon
        :rules="loginFormRules"
        ref="loginFormRef"
        label-width="70px"
      >
        <el-form-item label="用户名" prop="username">
          <el-input v-model="loginForm.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input
            v-model="loginForm.password"
            autocomplete="off"
            type="password"
          ></el-input>
        </el-form-item>
      </el-form>
      <!-- 底部按钮区 -->
      <span slot="footer" class="dialog-footer">
        <el-button @click="loginDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="Login">确 定</el-button>
      </span>
    </el-dialog>
    <el-backtop></el-backtop>
  </div>
</template>

<script>
import { mapState } from "vuex";
export default {
  data() {
    return {
      loginDialogVisible: false,
      loginForm: {
        username: "admin1",
        password: "123456"
      },
      loginFormRules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          {
            min: 3,
            max: 10,
            message: "用户名的长度在3-10个字符之间",
            trigger: "blur"
          }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          {
            min: 6,
            max: 15,
            message: "密码的长度在6-15个字符之间",
            trigger: "blur"
          }
        ]
      }
    };
  },
  asyncData(context) {
    // called every time before loading the component
  },
  fetch() {
    // The fetch method is used to fill the store before rendering the page
  },
  head() {
    // Set Meta Tags for this Page
  },
  // and more functionality to discover
  computed: {
    ...mapState({
      isLogin: state => state.isLogin //使用ES6的箭头函数来给count赋值
    })
  },
  methods: {
    async Login() {
      this.$refs.loginFormRef.validate(async vaild => {
        if (!vaild) {
          return;
        }
        const { data } = await this.$axios.get(
          "admin/getUser/" + this.loginForm.username
        );
        console.log(data)
        if (data.password === this.loginForm.password) {
          this.$message.success("登录成功!");
        } else {
          this.$message.error("登录失败!");
        }

        this.loginDialogVisible = false;
        // this.getUserList();
      });
    },
    loginDialogClosed() {
      this.$refs.loginFormRef.resetFields();
    }
  }
};
</script>

<style scoped>
.el-main {
  background-color: #f4f5f5;
}
.el-menu {
  /* margin: 0 20% 0; */
  width: 70%;
  margin: auto;
}
.box-card {
  width: 70%;
  margin: auto;
}
.right {
  float: right;
  /* background-color: #6699FF; */
  margin: auto;
  position: absolute;
  top: 50%;
  right: -6%;
  transform: translate(-50%, -50%);
}
</style>
