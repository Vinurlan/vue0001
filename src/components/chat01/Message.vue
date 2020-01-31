<template>
    <div class="message">
        <h3><p class="badge badge-secondary">{{mesprop.name}}</p></h3> <p>{{mesprop.message}}</p>
    </div>
</template>

<script>
export default {
    name: "Message",
    props: {
        mesprop: Object
    }
}
</script>

<style scoped>

.message {
    border: 1px solid rgba(155, 154, 154, 0.281);
    display: grid;
    grid-template-columns: 1fr 5fr;
}

p {
    text-align: left;
    margin: auto;
    margin-left: 0;
    word-break: break-all;
}

</style>