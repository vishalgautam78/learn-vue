<template>
    <div class="container mt-5"> 
        <div class="card">
            <div class="card-header">
                <h4>Edit Student</h4>
            </div>
            <div class="card-body">
                <div class="mb-3">
                    <label for="">Name</label>
                    <input type="text" v-model="model.student.name" :class="['form-control', errors.name ? 'is-invalid' : '']">
                    <div class="invalid-feedback">
                        {{ errors.name ? errors.name[0] : '' }}
                    </div>
                </div>
                <div class="mb-3">
                    <label for="">Email</label>
                    <input type="text" v-model="model.student.email" :class="['form-control', errors.email ? 'is-invalid' : '']">
                    <div class="invalid-feedback">
                        {{ errors.email ? errors.email[0] : '' }}
                    </div>
                </div>
                <div class="mb-3">
                    <label for="">Phone</label>
                    <input type="text" v-model="model.student.phone" :class="['form-control', errors.phone ? 'is-invalid' : '']">
                    <div class="invalid-feedback">
                        {{ errors.phone ? errors.phone[0] : '' }}
                    </div>
                </div>
                <div class="mb-3">
                    <button type="button" @click="updateStudent()" class="btn btn-primary float-end">Update</button>
                </div>
                <div>
                    <RouterLink to="/students" class="btn btn-secondary float-end">
                        Back
                    </RouterLink>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

    export default {
        name: 'StudentCreate',
        data() {
            return {
                studentId: '',
                model: {
                    student: {
                        name: '',
                        email: '',
                        phone: '',
                    },
                },
                errors: {}
            }
        },
        mounted() {
            this.getStudentData(this.$route.params.id);
            this.studentId = this.$route.params.id;
        },
        methods: {
            getStudentData(studentId) {
                axios.get(`http://127.0.0.1:8000/api/user/edit/${studentId}`).then(res => {
                    this.model.student = res.data;
                })
                .catch(function(error) {
                    if (error.response) {
                        if (error.response.status == 404) {
                            alert('No data found');
                        }
                    }
                })
            },
            async updateStudent() {
                try {
                    this.errors = {};
                    await axios.put(`http://127.0.0.1:8000/api/user/update/${this.studentId}`, this.model.student).then(res => {
                    alert(res.data.message);
                })
                } catch (error) {
                    if (error.response && error.response.status === 422) {
                        this.errors = error.response.data.errors;
                    }
                }
            }
        }
    }
</script>