<template>
    <div class="flex flex-row justify-between mt-4 mb-6">
        <div class="flex flex-row flex-none mr-8">
            <div class="rounded-xl mr-4 px-4" :style="color">{{status}}</div>
            <span class="px-2 mr-4 text-gray-400">{{count}}</span>
        </div>
        <div class="order-last flex flex-row text-gray-400">
            <nuxt-link :to="statusUrl">
                <button class="m-2 items-center justify-center" title="Edit Menu">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 12h.01M12 12h.01M19 12h.01M6 12a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0z" />
                    </svg>
                </button>
            </nuxt-link>
            <button class="m-2" title="Add New Status" @click="addStatus()">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
                </svg>
            </button>
        </div>
    </div>
</template>

<script>
export default {
    props: ['status', 'id', 'color', 'count', 'addStatus'],
    data () {
        return {
            statusUrl: `/statuses/${this.$props.id}`
        }
    }
}
</script>