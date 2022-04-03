<template>
    <div class="container">
        <div class="row">
            <div class="col">
                <img src="../assets/loading.gif" alt="" d-block m-auto>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Spinner"
}
</script>

<style scoped>
.container img{
    max-width: 80px;
    width: 100%;
    border-radius: 50%;
}
</style>