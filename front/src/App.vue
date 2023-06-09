<template>
  <div>
    <div class="col-8 offset-2">
      <table class="table caption-top table-hover">
        <caption class="text-center">
          <h1>Grades Management</h1>
          <el-button type="primary" icon="el-icon-search" @click="getStudent" class="a">Search</el-button>
          
          <el-button type="primary" @click="dialogVisibleLogin = true">Log in</el-button>
          <el-dialog
            title="Log In"
            :visible.sync="dialogVisibleLogin"
            width="30%"
            :before-close="handleCloseLog">
            <span>Username</span><input type="text" v-model="user_for_login.username"><br>
            <span>Password</span><input type="password" v-model="user_for_login.password">
            <span slot="footer" class="dialog-footer">
              <el-button @click="dialogVisibleLogin = false">Cancel</el-button>
              <el-button type="primary" @click="login">Login</el-button>
            </span>
          </el-dialog>

          <el-button type="primary" class="re" @click="dialogVisibleRegister = true">Register</el-button>

          <el-dialog
            title="Register"
            :visible.sync="dialogVisibleRegister"
            width="30%"
            :before-close="handleCloseReg">
            <span>Username</span><br><input type="text" v-model="user_for_register.username"><br>
            <span>Password</span><br><input type="password" v-model="user_for_register.password"><br>
            <span>Confirm Password</span><br><input type="password" v-model="user_for_register.password2">
            <span slot="footer" class="dialog-footer">
              <el-button @click="dialogVisibleRegister = false">Cancel</el-button>
              <el-button type="primary" @click="register">Register</el-button>
            </span>
          </el-dialog>

          <Add class="a"/>
        </caption>
        
        <thead>
          <tr>
            <th scope="col">Number</th>
            <th scope="col">Name</th>
            <th scope="col">Age</th>
            <th scope="col">Cal</th>
            <th scope="col">Econ</th>
            <th scope="col">CS</th>
            <th scope="col">Operation</th>
          </tr>
        </thead>
        <tbody>
          <Student v-for="stu in students_for_page" :key="stu.id" :student="stu"/>
        </tbody>
      </table>
      <div class="text-center">
        <el-button-group>
        <el-button type="primary" icon="el-icon-arrow-left" @click="lastPage">Previous Page</el-button>
        <el-button type="primary"  @click="nextPage">Next Page<i class="el-icon-arrow-right el-icon-right"></i></el-button>
      </el-button-group>
      </div>
      
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Student from './components/Student';
import Add from './components/Add.vue';
export default {
  name: 'App',
  components: {
    Student, Add
  },
  data() {
    return {
      page:1,
      students:[],
      dialogVisibleLogin: false,
      dialogVisibleRegister: false,
      user_for_login:{
        username:"",
        password:""
      },
      user_for_register:{
        username:"",
        password:"",
        password2:""
      }
    }
  },
  methods: {
    getStudent(){
      if (sessionStorage.getItem("isLogin") == "true"){
        this.page = 1,
        axios({
          url:"http://localhost:8080/students",
          method: 'GET'
        }).then( res => {
          this.students = res.data;
          console.log(res.data);
        })
      } else {
        this.$alert("Please log in.");
      }
    },
    nextPage(){
      this.page += 1;
    },
    lastPage(){

        this.page -= 1;
    },
    handleCloseLog(done) {
        this.$confirm('Are you sure not to log in?')
          .then(_ => {
            done();
          })
          .catch(_ => {});
    },
    handleCloseReg(done) {
        this.$confirm('Are you sure not to register?')
          .then(_ => {
            done();
          })
          .catch(_ => {});
    },
    login(){
      axios({
        url: "http://localhost:8080/login",
        method: 'POST',
        data: this.user_for_login
      }).then(res => {
        if (res.data==1){
          // sessionStorage 解决登陆状态维持
          sessionStorage.setItem("isLogin", true);
          this.dialogVisibleLogin = false
        } else {
          this.$alert("Wrong username or password");
        }
        
      })

    },
    register(){
      if (this.user_for_register.password == this.user_for_register.password2){
        axios({
          url: "http://localhost:8080/register",
          method: 'POST',
          data: this.user_for_register
        }).then(res => {
          if (res.data==1){
            // sessionStorage 解决登陆状态维持
            this.dialogVisibleRegister = false;
            this.$alert("Register Success!");
          } else {
            this.$alert("You already have an account!");
          }
        })
      } else {
        this.$alert("Passwords do not match!");
      }
    }
  },
  computed: {
    students_for_page(){
      return this.students.slice(this.page*5-5,this.page*5);
    }
  }
}
</script>

<style>
.re {
  margin-left: 10px;
}
</style>
