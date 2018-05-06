<template>
    <div>
        Login <input v-model="form.user"/> <br>
        Pass <input type="password" v-model="form.pass"><br>
        <button @click="login">Login</button>
    </div>
</template>

<script>
const blankForm = {
    user: '',
    pass: '',
}
export default {
    layout: 'public',
    data() {
        return {
            form: JSON.parse(JSON.stringify(blankForm))
        }
    },

    methods: {
        async login() {
            let res = await this.$http.post('/login', this.form)
            if (!res.data.ok) {
                // alert
                // focus pass
                return 
            } else {
                alert("OK")
            }
            window.sessionStorage.setItem('user', JSON.stringify({
                user: this.form.user,
                pass: this.form.pass,
            }))
            this.$router.push('/student')
        },
        reset() {
            this.form = JSON.parse(JSON.stringify(blankForm))
        },
    },
}
</script>
