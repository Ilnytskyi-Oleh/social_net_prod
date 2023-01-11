<template>
    <div class=" mx-auto max-w-xs space-y-4">
        <div>
            <input v-model="email" type="email" :placeholder="this.errors?.email ?? 'email'"
                   :class="['input input-bordered w-full ', this.errors?.email ? 'placeholder-red-900 border-red-800' : '']">
            <div v-if="this.errors?.email" class="alert alert-error shadow-lg mt-4">
                <div  v-for="pass in this.errors?.email">
                    <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current flex-shrink-0 h-6 w-6" fill="none" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
                    <span >{{ pass }}</span>
                </div>
            </div>
        </div>
        <div>
            <input v-model="password" type="password" :placeholder="this.errors?.password ?? 'password'"
                   :class="['input input-bordered w-full ', this.errors?.password ? 'placeholder-red-900 border-red-800' : '']">
            <div v-if="this.errors?.password" class="alert alert-error shadow-lg mt-4">
                <div  v-for="pass in this.errors?.password">
                    <svg xmlns="http://www.w3.org/2000/svg" class="stroke-current flex-shrink-0 h-6 w-6" fill="none" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>
                    <span >{{ pass }}</span>
                </div>
            </div>
        </div>
        <div class="text-right ">
            <button @click.prevent="login" type="submit" class="btn btn-primary ">Login</button>
        </div>
    </div>
</template>

<script>
export default {
    name: "Login",

    data() {
        return {
            email: null,
            password: null,
            errors:null,
        }
    },

    methods: {
        login() {
            axios.get('/sanctum/csrf-cookie')
                .then(response => {
                    axios.post('/login', {email: this.email, password: this.password})
                        .then(r => {
                            localStorage.setItem('x_xsrf_token', r.config.headers['X-XSRF-TOKEN'])
                            this.$router.push({name: 'user.personal'})
                        })
                        .catch(err => {
                            this.errors =  err.response.data.errors
                        })
                })
        }
    }
}
</script>

<style scoped>

</style>
