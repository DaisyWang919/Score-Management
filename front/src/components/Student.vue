<template>
  <tr>
      <td v-show="!is_edit">{{ student.number }}</td>
      <td v-show="!is_edit">{{ student.name }}</td>
      <td v-show="!is_edit">{{ student.age }}</td>
      <td v-show="!is_edit">{{ student.cal }}</td>
      <td v-show="!is_edit">{{ student.econ }}</td>
      <td v-show="!is_edit">{{ student.cs }}</td>
      <td v-show="!is_edit">
        <el-button type="primary" icon="el-icon-edit" circle @click="edit"></el-button>
        <el-button type="danger" icon="el-icon-delete" circle @click="remove"></el-button>
      </td>

      <td v-show="is_edit"><input type="text" v-model="student.number" class="in"/></td>
      <td v-show="is_edit"><input type="text" v-model="student.name" class="in"/></td>
      <td v-show="is_edit"><input type="text" v-model.number="student.age" class="in"/></td>
      <td v-show="is_edit"><input type="text" v-model.number="student.cal" class="in"/></td>
      <td v-show="is_edit"><input type="text" v-model.number="student.econ" class="in"/></td>
      <td v-show="is_edit"><input type="text" v-model.number="student.cs" class="in"/></td>
      <td v-show="is_edit">
        <el-button type="success" icon="el-icon-check" circle @click="save"></el-button>
        <el-button type="danger" icon="el-icon-delete" circle @click="remove"> </el-button>
      </td>
  </tr>
</template>

<script>
import axios from "axios";
export default {
    props: ["student"],
    data() {
        return {
            is_edit: false
        }
    },
    methods: {
        edit(){
            this.is_edit = true
        },
        save(){
            axios({
                url: "http://localhost:8080/update",
                method: 'POST',
                data: this.student
            }),
            this.is_edit = false
        },
        remove(){
            axios({
                url: "http://localhost:8080/delete",
                method: 'POST',
                data: this.student
            }),
            location.reload()
        }
    }
}
</script>

<style>
.in {
    width: 60px;
}
</style>