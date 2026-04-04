<template>
    <div class="container mt-5"> 
        <div class="card">
            <div class="card-header">
                <h4>Add Student</h4>
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
                    <label for="">Profile Photo</label>
                    <input type="file" @change="handleFileUpload" :class="['form-control', errors.image ? 'is-invalid' : '']">
                    <div class="invalid-feedback">
                        {{ errors.image ? errors.image[0] : '' }}
                    </div>
                </div>
                <!-- Image Preview -->
                <div v-if="previewImage" class="mt-3">
                    <img :src="previewImage" width="120" height="120" style="object-fit: cover; border-radius: 8px;">
                </div>
                <div>
                    <div class="mb-3">
                        <button type="button" @click="saveStudent" class="btn btn-primary float-end">Save</button>
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
                model: {
                    student: {
                        name: '',
                        email: '',
                        phone: '',
                        image: null,
                    },
                },
                errors: {},
                previewImage: null,
            }
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
            async saveStudent() {
                try {
                    this.errors = {};
                    await axios.post('http://127.0.0.1:8000/api/user/create', this.model.student).then(res => {
                    alert(res.data.message);
                    this.model.student = {};
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