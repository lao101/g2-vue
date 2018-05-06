<template>
    <div>
        <h1 class="red--text">Student Module</h1>
        <button @click="logout">LOGOUT</button>
        <nuxt-child />
    </div>
</template>

<script>
export default {
    methods: {
        logout() {
            window.sessionStorage.removeItem('user')
            this.$router.replace('/login')
        }
    }
}
</script>

