<template>
    <div class="mx-6 my-4">
        <status-header :status="title" :count="cards.length" :addStatus="addStatusFn" :color="color" :id="$vnode.key"/>
        <div class="overflow-y-auto h-96">
            <card v-for="card in cards" :key="card" :content="getCard(card)" :status="$vnode.key"/>
            <new-card @click.native="addCard()"/>
        </div>
    </div>
</template>

<script>
import Card from './Card.vue';
import NewCard from './NewCard.vue';
import StatusHeader from './StatusHeader.vue';

import { v4 as uuidv4 } from 'uuid';

export default {
    components: { StatusHeader, Card, NewCard },
    props: ['addStatusFn'],
    mounted() {
        let statusData = JSON.parse(localStorage.getItem(this.$vnode.key));
        this.title = statusData.title;
        this.cards = statusData.cards;
        this.color.backgroundColor = statusData.color;
    },
    data() {
        return {
            cards: [],
            title: '',
            color: {
                backgroundColor: ''
            }
        }
    },
    methods: {
        addCard() {
            let newCard = {
                title: 'Untitled Card',
                desc: 'task description',
                parent: this.$vnode.key
            }
            let uuid = uuidv4();
            localStorage.setItem(uuid, JSON.stringify(newCard));
            this.cards.push(uuid);
            let statusData = JSON.parse(localStorage.getItem(this.$vnode.key));
            statusData.cards = this.cards;
            localStorage.setItem(this.$vnode.key, JSON.stringify(statusData));
        },
        getCard(uuid) {
            let cardData = JSON.parse(localStorage.getItem(uuid));
            return cardData.title;
        }
    }
}
</script>
