<template>
    <div class="text-center">
        <h1 class="title">
            Edit Status
        </h1>
        <div class="container flex-col items-center">
        <input class="mb-4 p-2 font-extrabold text-5xl text-center" v-model="title" placeholder="title">
        <input class="mb-8 p-2 rounded-md text-center" v-model="color" placeholder="color">
        <div>
            <button @click="updateStatus" class="bg-green-700 text-white text-lg mt-8 px-4 py-2 rounded-lg transition hover:bg-green-900">
                save
            </button>
            <button @click="deleteStatus" class="bg-red-700 text-white text-lg mt-8 px-4 py-2 rounded-lg transition hover:bg-red-900">
                delete
            </button>
            <nuxt-link to="/">
            <button class="bg-gray-700 text-white text-lg mt-8 px-4 py-2 rounded-lg transition hover:bg-gray-900">
                cancel
            </button>
            </nuxt-link>
        </div>
        </div>
    </div>
</template>

<script>
export default {
    mounted() {
        let statusData = JSON.parse(localStorage.getItem(this.$route.params.status));
        this.title = statusData.title;
        this.color = statusData.color;
        this.cards = statusData.cards;  // just to save a getItem call on update phase
    },
    data() {
        return {
            cards: [],
            title: '',
            color: ''
        }
    },
    methods: {
        updateStatus() {
            let status = {
                title: this.title,
                color: this.color,
                cards: this.cards
            }
            localStorage.setItem(this.$route.params.status, JSON.stringify(status));
            this.$router.push({
                path: '/'
            });
        },
        deleteStatus() {
            this.cards.forEach(card => {
                localStorage.removeItem(card);
            });
            localStorage.removeItem(this.$route.params.status);
            let statuses = JSON.parse(localStorage.getItem('statuses'));
            statuses.splice(statuses.indexOf(this.$route.params.status),1);
            localStorage.setItem('statuses', JSON.stringify(statuses));
            this.$router.push({
                path: '/'
            });
        }
    }
}
</script>