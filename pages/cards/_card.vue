<template>
    <div class="text-center">
        <h1 class="title">
            Edit Card
        </h1>
        <div class="container flex-col items-center">
        <input class="m-4 p-2 font-extrabold text-5xl text-center" :class="pickingStatus ? 'opacity-60' : ''" v-model="title" placeholder="title">
        <button @click="() => this.pickingStatus = true" class="m-4 mb-8 p-2 rounded-md" :class="pickingStatus ? 'opacity-60' : ''" :style="parentColor">{{parent}}</button>
        <textarea placeholder="task description" class="text-center h-64" :class="pickingStatus ? 'opacity-60' : ''" v-model="description"></textarea>
        <status-picker v-if="pickingStatus" :alterStatus="scheduleMove" :statuses="statusList" :toggle="() => this.pickingStatus = false" class="bg-white shadow-2xl"/>
        <div :class="pickingStatus ? 'opacity-60' : ''">
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
import StatusPicker from '~/components/StatusPicker.vue';
export default {
  components: { StatusPicker },
    data() {
        return {
            title: '',
            description: '',
            status: '',
            parent: '',
            parentColor: {
                backgroundColor: ''
            },
            pickingStatus: false,
            statusList: [],
            moveTo: ''
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
        this.statusList = JSON.parse(localStorage.getItem('statuses')) || [];
    },
    methods: {
        updateCard() {
            if(this.moveTo.length != 0) {
                this.moveCard(this.moveTo);
            } else {
                let cardData = {
                    title: this.title,
                    description: this.description,
                    parent: this.status
                }
                localStorage.setItem(this.$route.params.card, JSON.stringify(cardData));
            }
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
        },
        scheduleMove(dest) {
            this.moveTo = dest;
            let newStatus = JSON.parse(localStorage.getItem(dest));
            this.parent = newStatus.title;
            this.parentColor.backgroundColor = newStatus.color;
        },
        moveCard(dest) {
            let cardData = {
                title: this.title,
                description: this.description,
                parent: dest
            }
            localStorage.setItem(this.$route.params.card, JSON.stringify(cardData));
            let oldStatus = JSON.parse(localStorage.getItem(this.status));
            oldStatus.cards.splice(oldStatus.cards.indexOf(this.$route.params.card),1);
            localStorage.setItem(this.status, JSON.stringify(oldStatus));
            let newStatus = JSON.parse(localStorage.getItem(cardData.parent));
            newStatus.cards.push(this.$route.params.card);
            localStorage.setItem(cardData.parent, JSON.stringify(newStatus));
            this.parent = newStatus.title;
            this.parentColor.backgroundColor = newStatus.color;
        }
    }
}
</script>