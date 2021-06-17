<template>
    <div class="flex flex-row justify-between mt-4 mb-6">
        <div class="flex flex-row flex-none mr-8">
            <div class="rounded-xl mr-4 px-4" :style="color">{{status}}</div>
            <span class="px-2 mr-4 text-gray-400">{{count}}</span>
        </div>
        <div class="order-last flex flex-row text-gray-400">
                <div @click="() => this.editMenu = !this.editMenu">
                    <button class="m-2 items-center justify-center" title="Edit Menu">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 12h.01M12 12h.01M19 12h.01M6 12a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0z" />
                        </svg>
                    </button>
                    <div :class="editMenu ? '' : 'hidden'" class="z-50 absolute border-2 bg-white text-black text-lg rounded-xl flex flex-col">
                        <nuxt-link :to="statusUrl">
                            <button title="edit" class="transition hover:bg-gray-300 min-w-full py-2 px-6 rounded-xl">
                                edit
                            </button>
                        </nuxt-link>
                        <button title="delete" @click="deleteStatus()" class="text-red-700 transition hover:bg-red-700 hover:text-white py-2 px-6 rounded-xl">
                            delete
                        </button>
                    </div>
                </div>
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
            statusUrl: `/statuses/${this.$props.id}`,
            editMenu: false
        }
    },
    methods: {
        deleteStatus() {
            let cards = JSON.parse(localStorage.getItem(this.$props.id)).cards;
            cards.forEach(card => {
                localStorage.removeItem(card);
            });
            localStorage.removeItem(this.$props.id);
            let statuses = JSON.parse(localStorage.getItem('statuses'));
            statuses.splice(statuses.indexOf(this.$props.id),1);
            localStorage.setItem('statuses', JSON.stringify(statuses));
            this.$router.go();
        }
    }
}
</script>