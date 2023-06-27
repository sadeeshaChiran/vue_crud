
<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, Link, router } from '@inertiajs/vue3';
</script>

<template>
    <Head title="Dashboard" />

    <div>

        <div class="container">
            <div class="d-flex justify-content-center row bg-dark-subtle">
                <div class="mt-4 mb-4 col-lg-10">
                    <div class='row'>


                        <Link :href="route('dashboard')" class="col-6 fs-2 text-start fw-bold text-success page-title">
                        Welcome To The Dashboard</Link>

                        <Link :href="route('Welcome')" class="mt-2 col-6 fs-4 text-end text-danger page-title">Back</Link>



                    </div>

                </div>
            </div>
            <div class='mt-5 col-10 offset-2'>

                <button class='mt-2 mb-5 col-9 btn btn-primary' data-bs-toggle="modal"
                    data-bs-target="#addModal">ADD</button>


                <div class='row'>



                    <div v-for="student in students" class="m-1 mt-3 col-3 card">
                        <img :src='student.image' class="card-img-top" alt="url_problem" />
                        <div class="text-center card-body">
                            <span class="mt-2 card-text fs-3 text-info">{{ student.name }}</span>
                            <br />
                            <span class="mt-2 card-text fs-3 text-success">{{ student.age }}</span>
                            <br />

                            <span class="mt-2 card-text fs-3">{{ student.status }}</span>

                            <button @click.prevent="changeStatus(student.id)" class='mt-2 col-12 btn btn-outline-danger'>
                                change
                            </button>
                            <button @click.prevent="editStatus(student.id, student.name, student.age)"
                                data-bs-toggle="modal" data-bs-target='#editData'
                                class='mt-2 col-12 btn btn-outline-primary'>
                                Edit
                            </button>

                            <button @click.prevent="deleteStudent(student.id)" class='mt-2 col-12 btn btn-danger'>
                                Delete
                            </button>




                        </div>
                    </div>

                </div>

            </div>

            <!-- add student model -->
            <div class="modal " id="addModal" tabIndex={-1}>
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h1 class="modal-title fs-5" id="addModalLabel">Modal title</h1>

                        </div>
                        <div class="modal-body">
                            <form @submit.prevent="submit">

                                <div class="mb-3">
                                    <label htmlFor="name" class="form-label">Name</label>
                                    <input id='name' name='name' v-model="student.name" type="text" class="form-control"
                                        aria-describedby="emailHelp" required />
                                </div>
                                <div class="mb-3">
                                    <label htmlFor="age" class="form-label">Age</label>
                                    <input id='age' name='age' v-model="student.age" type="number" class="form-control"
                                        required />
                                </div>


                                <div class='mb-3'>
                                    <div class="mt-2 offset-lg-1 col-lg-10 ">

                                        <input type="file" name="image" class='col-lg-10 form-control '
                                            @change="handleImageChange" accept="image/*" required />


                                    </div>
                                </div>
                                <button type="submit" class="mt-4 btn btn-primary col-12 d-grid bg-primary ">Submit</button>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary bg-secondary"
                                data-bs-dismiss="modal">Close</button>

                        </div>
                    </div>
                </div>
            </div>


            <!-- edit model -->
            <div class="modal fade " id="editData" tabIndex={-1} aria-labelledby="editModalLabel" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h1 class="modal-title fs-5" id="editModalLabel">Edit</h1>

                        </div>
                        <div class="modal-body">
                            <form @submit.prevent="update">

                                <div class="mb-3">
                                    <label htmlFor="name" class="form-label">Name</label>
                                    <input id='name' name='name' v-model="studentUpdate.name" type="text"
                                        class="form-control" aria-describedby="emailHelp" required />
                                </div>

                                <div class="mb-3">
                                    <label htmlFor="age" class="form-label">Age</label>
                                    <input id='age' name='age' v-model="studentUpdate.age" type="number"
                                        class="form-control" required />
                                </div>

                                <button type="submit" class="mt-4 btn btn-primary col-12 d-grid bg-primary ">Submit</button>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button onClick={handleReset} type="button" class="btn btn-secondary bg-secondary"
                                data-bs-dismiss="modal">Close</button>

                        </div>
                    </div>
                </div>
            </div>




        </div>
    </div>
</template>

<script>


export default {
    props: {
        students: Array
    },
    data() {
        return {
            student: {
                name: null,
                age: null,
                image: null,
            },
            studentUpdate: {
                name: null,
                age: null,
                id: null,
            }
        };
    },
    methods: {
        handleImageChange(event) {
            this.image = event.target.files[0];
        },
        submit() {
            const formData = new FormData();
            formData.append("image", this.image);
            formData.append("name", this.student.name);
            formData.append("age", this.student.age);
            this.$inertia.post("/add", formData, {
                headers: {
                    "Content-Type": "multipart/form-data",
                },
            });
            this.student.name = null;
            this.student.age = null;
            this.student.image = null;
        },
        deleteStudent(id) {
            this.$inertia.get(this.route("student.delete", { id: id }));
        },
        changeStatus(id) {
            this.$inertia.get(this.route("student.status", { id: id }));
        },
        editStatus(id, name, age) {
            this.studentUpdate.name = name;
            this.studentUpdate.age = age;
            this.studentUpdate.id = id;
        },
        update() {
            router.post("/update", this.studentUpdate);
            this.studentUpdate.name = "";
            this.studentUpdate.age = "";
        }
    },
    components: { Link }
}






</script>
