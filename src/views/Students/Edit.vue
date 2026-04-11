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
                <div>
                    <!-- Show Old Image -->
                    <img 
                        v-if="previewImage" 
                        :src="previewImage" 
                        width="120"
                    />

                    <!-- File Input -->
                    <input type="file" @change="handleFileUpload" />
                </div>
                <div>
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
                        image: null
                    },
                },
                errors: {},
                previewImage: null,
            }
        },
        mounted() {
            this.getStudentData(this.$route.params.id);
            this.studentId = this.$route.params.id;
        },
        methods: {
            handleFileUpload(event) {
                const file = event.target.files[0];
                if (!file) {
                    return;
                }
                // Image Validation
                if (!file.type.startsWith('image/')) {
                    alert('Only image files are allowed');
                    return;
                }
                if (file.size > 2 * 1024 * 1024) {
                    alert('Max file size is 2MB');
                    return;
                }
                this.model.student.image = file;
                // Preview
                this.previewImage = URL.createObjectURL(file);
            },
            getStudentData(studentId) {
                axios.get(`http://127.0.0.1:8000/api/user/edit/${studentId}`).then(res => {
                    console.log('vishal', res.data.image);
                    this.model.student = res.data;
                    // Show existing image
                    this.previewImage = `http://127.0.0.1:8000/storage/${res.data.image}`;
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
                    let formdata = new FormData();
                    formdata.append('_method', 'PUT');
                    formdata.append('name', this.model.student.name);
                    formdata.append('email', this.model.student.email);
                    formdata.append('phone', this.model.student.phone);
                    if (this.model.student.image) {
                        formdata.append('image', this.model.student.image);
                    }
                    await axios.post(`http://127.0.0.1:8000/api/user/update/${this.studentId}`, formdata, { headers: { 'Content-Type': 'multipart/form-data' } }).then(res => {
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