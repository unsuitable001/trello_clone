<template>
    <div class="text-center">
        <h1 class="title">
            Edit Card
        </h1>
        <div class="container flex-col items-center">
        <input class="m-4 p-2 font-extrabold text-5xl text-center" v-model="title" placeholder="title">
        <div class="m-4 mb-8 p-2 rounded-md" :style="parentColor">{{parent}}</div>
        <textarea placeholder="task description" class="text-center h-64" v-model="description"></textarea>
        <div>
            <button @click="updateCard()" class="bg-green-700 text-white text-lg mt-8 px-4 py-2 rounded-lg transition hover:bg-green-900">
                save
            </button>
            <button @click="deleteCard()" class="bg-red-700 text-white text-lg mt-8 px-4 py-2 rounded-lg transition hover:bg-red-900">
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
    data() {
        return {
            title: '',
            description: '',
            status: '',
            parent: '',
            parentColor: {
                backgroundColor: ''
            }
        }
    },
    mounted() {
        let cardData = JSON.parse(localStorage.getItem(this.$route.params.card));
        this.title = cardData.title;
        this.description = cardData.description;
        this.status = cardData.parent;
        let statusData = JSON.parse(localStorage.getItem(cardData.parent));
        this.parent = statusData.title;
        this.parentColor.backgroundColor = statusData.color;
    },
    methods: {
        updateCard() {
            let cardData = {
                title: this.title,
                description: this.description,
                parent: this.status
            }
            localStorage.setItem(this.$route.params.card, JSON.stringify(cardData));
            this.$router.push({
                path: '/'
            });
        },
        deleteCard() {
            localStorage.removeItem(this.$route.params.card);
            let statusData = JSON.parse(localStorage.getItem(this.status));
            statusData.cards.splice(statusData.cards.indexOf(this.$route.params.card),1);
            localStorage.setItem(this.status, JSON.stringify(statusData));
            this.$router.push({
                path: '/'
            });
        }
    }
}
</script>