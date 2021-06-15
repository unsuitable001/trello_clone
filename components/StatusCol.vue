<template>
    <div class="mx-6 my-4">
        <status-header :status="title" :count="cards.length"/>
        <div class="overflow-y-auto h-96">
            <card v-for="(card, idx) in cards" :key="idx" :content="card" :status="$vnode.key"/>
            <new-card @click.native="addCard()"/>
        </div>
    </div>
</template>

<script>
import Card from './Card.vue';
import NewCard from './NewCard.vue';
import StatusHeader from './StatusHeader.vue';

export default {
    components: { StatusHeader, Card, NewCard },
    mounted() {
        let statusData = JSON.parse(localStorage.getItem(1));
        this.title = statusData.title;
        this.cards = statusData.cards;
    },
    data() {
        return {
            cards: [],
            title: '',
            color: ''
        }
    },
    methods: {
        addCard() {
            this.cards = [...this.cards, 'Untitled Card'];
            // let statusData = JSON.parse(localStorage.getItem(1));
        }
    }
}
</script>
