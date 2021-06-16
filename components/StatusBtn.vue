<template>
    <button @click="changeStatus()" class="m-2 p-2 transition hover:bg-gray-300">
        {{title}}
    </button>
</template>

<script>
export default {
    props: ['changeStatus'],
    data() {
        return {
            title: '',
        }
    },
    mounted() {
        let status = JSON.parse(localStorage.getItem(this.$vnode.key));
        this.title = status.title;
    }
}
</script>