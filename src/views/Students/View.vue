<template>
  <main>
    <div class="container mt-2">
      <div class="card">
      <div class="card-header">
        <h4>
          Students
          <RouterLink to="/student/create" class="btn btn-primary float-end">
            Add Student
          </RouterLink>
        </h4>
      </div>
      <div class="card-body">
        <table class="table table-border">
          <thead>
            <tr>
              <th>ID</th>
              <th>Image</th>
              <th>Name</th>
              <th>Email</th>
              <th>Phone</th>
              <th>Created At</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody v-if="this.students.length > 0">
            <tr v-for="(student, index) in this.students" :key="index">
              <td>{{ student.id }}</td>
              <td> <img :src="getImgUrl(student.image)" width="80" alt=""> </td>
              <td>{{ student.name }}</td>
              <td>{{ student.email }}</td>
              <td>{{ student.phone }}</td>
              <td>{{ formatDate(student.created_at) }}</td>
              <td>
                <RouterLink :to="{path: '/student/edit/'+student.id}" class="btn btn-success">Edit</RouterLink>
                <button type="button" @click="deleteUser(student.id)" class="btn btn-danger ms-1">Delete</button>
              </td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr>
              <td colspan="7">Loading</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    </div>
  </main>
</template>

<script>
import axios from 'axios';

  export default {
    name: 'students',
    data() {
      return {
        students: [],
      }
    },
    mounted() {
      this.getUsers();
    },
    methods: {
      getUsers() {
        axios.get('http://127.0.0.1:8000/api/users').then(res => {
          this.students = res.data;
        })
      },
      formatDate(date) {
        return new Date(date).toLocaleDateString('en-GB', {
          day: '2-digit',
          month: 'short',
          year: 'numeric'
        });
      },
      deleteUser(id) {
        if (confirm('Are you sure want to delete this data?')) {
          axios.delete(`http://127.0.0.1:8000/api/user/destroy/${id}`).then(res => {
            alert(res.data.message);
            this.getUsers();
          }); 
        }
      },
      getImgUrl(image) {
        return `http://127.0.0.1:8000/storage/${image}`
      }
    }
  }
</script>
