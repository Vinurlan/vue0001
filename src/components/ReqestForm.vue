<template>
    <div class="form-box">
        <form class="form-request">
            <input type="text" placeholder="Name">
            <input type="number" placeholder="Phone">
            <input type="text" placeholder="Comment"> 
            <button @click.prevent>Отправить</button>
            <button @click.prevent="close">Закрыть</button>
        </form>
    </div>
</template>

<script>

export default {
    name: "ReqestForm",
    props: {
        close: Function
    }
}

</script>

<style scoped>
.form-request {
    position: fixed;
    top: 40%;
    left: 38%;
    background-color: #fff;
    border: 1px solid #000;
    width: 200px;
    height: 200px;
}

input {
    margin: 10px 0;
}

button {
    margin-top: 20px;
}

</style>