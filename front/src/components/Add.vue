<template>
    <div>
        <el-button type="text" @click="dialogVisible = true">Click to add a new student</el-button>

        <el-dialog
        title="New Student"
        :visible.sync="dialogVisible"
        width="30%"
        :before-close="handleClose">
        <div>
            Enter the info below:
        </div>
        <span>Number</span><input type="text" v-model="newStudent.number"/><br>
        <span>Name</span><input type="text" v-model="newStudent.name"/><br>
        <span>Age</span><input type="text" v-model.number="newStudent.age"/><br>
        <span>Cal</span><input type="text" v-model.number="newStudent.cal"/><br>
        <span>Econ</span><input type="text" v-model.number="newStudent.econ"/><br>
        <span>CS</span><input type="text" v-model.number="newStudent.cs" /><br>
        <span slot="footer" class="dialog-footer">
            <el-button @click="dialogVisible = false">Cancel</el-button>
            <el-button type="primary" @click="addStudent">Confirm</el-button>
        </span>
        </el-dialog>
    </div>
</template>

<script>
import axios from 'axios';

  export default {
    data() {
      return {
        dialogVisible: false,
        newStudent: {
            number:"",
            name:"",
            age:"",
            cal:"",
            econ:"",
            cs:"",
        }
      };
    },
    methods: {
      handleClose(done) {
        this.$confirm('Are you sure not to save this student?')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      addStudent(){
        axios({
            url: "http://localhost:8080/add",
            method: 'POST',
            data: this.newStudent
        }),
        this.dialogVisible = false,
        location.reload()
      }
    }
  };
</script>
