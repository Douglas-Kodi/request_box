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
    body{
        background-image: url('/img/background.jpg');
    }
    .main{
        min-height:712px;
        margin:25px;
        background-color:rgba(0, 0, 0, 0.5);
    }
</style>

