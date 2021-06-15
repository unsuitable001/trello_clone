<template>
    <div class="container items-center flex-col">
        <div contenteditable="true" class="m-6 mb-8 p-2 font-extrabold text-5xl">Untitled Card</div>
        <textarea placeholder="task description" class="text-center"></textarea>
        <div class="m-6 mb-8 p-2 bg-red-300 rounded-md min-h-64">Not Started</div>
        <button class="bg-green-700 text-white text-lg mt-8 px-4 py-2 rounded-lg transition hover:bg-green-900">
            save
        </button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            title: '',
            description: '',
            status: ''
        }
    },
    mounted() {
        
    }
    // {{$route.params.card}}
}
</script>