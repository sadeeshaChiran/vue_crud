<script setup>
import { Head, Link } from '@inertiajs/vue3';

defineProps({
    canLogin: {
        type: Boolean,
    },
    canRegister: {
        type: Boolean,
    },
    laravelVersion: {
        type: String,
        required: true,
    },
    phpVersion: {
        type: String,
        required: true,
    },
});
</script>

<template>
    <Head title="Welcome" />

    <div className="container">
        <div className="d-flex justify-content-center row bg-dark-subtle">
            <div className="mt-4 mb-4 col-lg-10">
                <div v-if="canLogin" className='row'>
                    <div v-if="$page.props.auth.user" class=" col-12">
                        <Link :href="route('dashboard')" className="col-10 fs-2 text-start text-success page-title">Go Dashboard ->
                        </Link>
                    </div>

                    <div class="col-12" v-else>
                        <div class="row">
                            <div class=" col-10">
                                <Link :href="route('Welcome')"  class="col-10 fs-2 text-start page-title">Signing Page</Link>
                            </div>


                            <div v-if="canLogin" className='mt-2 col-2 text-end'>

                                <div className='row'>
                                    <Link :href="route('login')"
                                        className="col-6 fs-5 text-end page-title fw-lighter text-success">
                                    Login
                                    </Link>
                                    <Link v-if="canRegister" :href="route('register')"
                                        className="col-6 fs-5 text-end page-title fw-lighter text-success">
                                    Register
                                    </Link>
                                </div>

                            </div>
                        </div>

                    </div>


                </div>

            </div>
        </div>

    </div>
</template>

<style>
.bg-dots-darker {
    background-image: url("data:image/svg+xml,%3Csvg width='30' height='30' viewBox='0 0 30 30' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1.22676 0C1.91374 0 2.45351 0.539773 2.45351 1.22676C2.45351 1.91374 1.91374 2.45351 1.22676 2.45351C0.539773 2.45351 0 1.91374 0 1.22676C0 0.539773 0.539773 0 1.22676 0Z' fill='rgba(0,0,0,0.07)'/%3E%3C/svg%3E");
}

@media (prefers-color-scheme: dark) {
    .dark\:bg-dots-lighter {
        background-image: url("data:image/svg+xml,%3Csvg width='30' height='30' viewBox='0 0 30 30' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1.22676 0C1.91374 0 2.45351 0.539773 2.45351 1.22676C2.45351 1.91374 1.91374 2.45351 1.22676 2.45351C0.539773 2.45351 0 1.91374 0 1.22676C0 0.539773 0.539773 0 1.22676 0Z' fill='rgba(255,255,255,0.07)'/%3E%3C/svg%3E");
    }
}
</style>
