<template>
    <div class="container">
        <div class="message-container">
            <p>{{ msg }}</p>
        </div>
    </div>
</template>

<script>
export default {
    name: "Message",
    props: {
        msg: String
    }
}
</script>

<style scoped>
    .container{
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .message-container{
        color:#004085;
        background-color: #CCE5FF;
        border: 2px solid #B8DAFF;
        border-radius: 5px;
        padding:10px;
        max-width:80%;
        margin:30px auto;
        align-content: center;
        position:absolute;
    }
</style>