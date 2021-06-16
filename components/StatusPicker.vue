<template>
    <div @click="toggle()" class="fixed pin z-50 overflow-auto flex">
        <div class="relative p-8 bg-white w-full max-w-md m-auto flex-col flex">
            <status-btn v-for="status in statuses" :key="status" :changeStatus="() => alterStatus(status)"/>
        </div>
    </div>
</template>

<script>
import StatusBtn from './StatusBtn.vue'
export default {
  components: { StatusBtn },
    props: ['toggle', 'statuses', 'alterStatus'],
    data() {
        return {
        }
    },
    methods: {
    }
}
</script>