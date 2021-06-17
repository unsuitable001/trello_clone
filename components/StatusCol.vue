<template>
    <div class="mx-6 my-4"
    @drop="onDrop($event, $vnode.key)"
    @drop.prevent
    @dragenter.prevent 
    @dragover.prevent>
        <status-header :status="title" :count="cards.length" :addStatus="addStatusFn" :color="color" :id="$vnode.key"/>
        <div class="overflow-y-auto h-96">
            <card v-for="card in cards" :key="card" :content="getCard(card)" 
            :status="$vnode.key" draggable="true" 
            @dragstart.native="startDrag($event, card)"
            @dragend.native="cancelMove($event, card)" />
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
        },
        startDrag(event, card) {
            event.dataTransfer.dropEffect = 'move';
            event.dataTransfer.effectAllowed = 'move';
            event.dataTransfer.setData('cardID', card);
            event.dataTransfer.setData('oldStatus', this.$vnode.key);
            setTimeout(() => {this.cards.splice(this.cards.indexOf(card),1);}, 0);
            console.log(card);
        },
        onDrop(event, status) {
            // console.log(status);
            let card = event.dataTransfer.getData('cardID');
            let oldStatus = event.dataTransfer.getData('oldStatus');
            console.log(card)
            this.moveCard(card, oldStatus, status);

        },
        moveCard(card, src, dest) {
            if(src == dest) return;
            console.log(card)
            // console.log(src)
            // console.log(dest)
            this.cards.push(card);
            let cardData = JSON.parse(localStorage.getItem(card));
            cardData.parent = dest;
            localStorage.setItem(card, JSON.stringify(cardData));

            let oldStatus = JSON.parse(localStorage.getItem(src));
            oldStatus.cards.splice(oldStatus.cards.indexOf(card),1);
            localStorage.setItem(src, JSON.stringify(oldStatus));

            let newStatus = JSON.parse(localStorage.getItem(dest));
            newStatus.cards.push(card);
            localStorage.setItem(dest, JSON.stringify(newStatus));
        },
        cancelMove(event, card) {
            let oldStatus = event.dataTransfer.getData('oldStatus');
            let cardData = JSON.parse(localStorage.getItem(card));
            if(cardData.parent == oldStatus) {
                this.cards.push(card);
            }
        }
    }
}
</script>
