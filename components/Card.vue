<template>
<!-- patch for chrome -->
    <div>
        <nuxt-link :to="cardUrl">
            <div class="border-2 rounded-md text-left py-2 px-4 my-2">
                
                <div>{{content}}</div>
                
            </div>
        </nuxt-link>
    </div>
        
</template>

<script>
export default {
    props: ['content', 'status'],
    data() {
        return {
            cardUrl: `/cards/${this.$vnode.key}`
        }
    }
}
</script>