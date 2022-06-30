<template>
    <div id="nav">
        <router-link to="/" id="logo-url">
            <img :src="logo" :alt="alt" id="logo">
        </router-link>
        <router-link to="/">Home</router-link>
        <router-link to="/List">List</router-link>
    </div>
</template>

<script>
export default {
    name: "Navbar",
    props: ["logo", "alt"]
}
</script>

<style scoped>
    #nav{
        width:100%;
        height:75px;
        background-color:rgb(134, 130, 130);
        display:flex;
        justify-content:flex-end;
        align-items:center;
    }
    #nav a{
        padding:0px 20px;
        font-size:20px;
        color:black;
        transition:.5s;
    }
    #nav a:hover{
        color:white;
    }
    #logo-url{
        margin:auto;
        margin-left:0;
    }
    #logo{
        width:70px;
        height:70px;
        padding:5px;
    }
</style>