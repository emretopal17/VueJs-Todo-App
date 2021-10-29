<template>
    <div class="row ">
        <div class="col-6 offset-3 border d-flex justify-content-between align-items-center pt-2 pb-2 bg-white">
            <span> {{todo.text }} </span>
            <button @click="$emit('deleteTodo',todo.id)" class="btn btn-danger">Sil</button>
        </div>
    </div>
</template>

<script>
export default {
    props : ["todo"]
}
</script>