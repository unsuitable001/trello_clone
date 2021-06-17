<template>
    <div @click="toggle()" class="z-50 absolute border-2 rounded-xl">
        <div class="relative bg-white w-full max-w-md m-auto flex-col flex">
            <status-btn v-for="status in statuses" :key="status" :changeStatus="() => alterStatus(status)"/>
        </div>
    </div>
</template>

<script>
import StatusBtn from './StatusBtn.vue'
export default {
  components: { StatusBtn },
    props: ['toggle', 'statuses', 'alterStatus']
}
</script>