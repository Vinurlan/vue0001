<template>
    <div class="request">
        <span class="request-name">Name: {{list.name}}</span>
        <span class="request-number">Phone: {{list.number}}</span>
        <span class="request-comment">Comment: {{list.comment}}</span>
    </div>
</template>

<script>
export default {
    name: "PropList",
    props: {
        list: Object
    },
    data() {
        return {

        }
    }
}
</script>

<style scoped>

.request{
    border-bottom: 1px solid #000;
    margin: 10px 0;
    display: grid;
    grid-template-rows: 1fr
}

.request-name{
    font-size: 14pt;
}

</style>