<template>
    <Navbar :logo="logo_src" :alt="alt_name" />
    <router-view />
    <Footer />
</template>

<script>
import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'

export default {
    components: { 
        Navbar,
        Footer
    },
    data() {
        return {
            logo_src: "/img/logo.png",
            alt_name: "Game"
        }
    }
}
</script>

<style>
    *{
        margin:0;
        padding:0;
        text-decoration: none;
        font-family: sans-serif;
        box-sizing: border-box;
    }
    .main{
        min-height:300px;
        margin:50px;
    }
</style>

