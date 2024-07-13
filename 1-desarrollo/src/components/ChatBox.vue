<template>
    <div class="container mb-3">
        <div class="d-flex flex-column" v-for="(message, index) in messages"
            :class="message.userId == firstUser.id.value ? 'align-items-start' : message.userId == secondUser.id.value ? 'align-items-end' : ''"
            :key="index">
            <small class="time fw-light text-secondary mb-1">{{ message?.name }} {{ message?.last }}</small>
            <div class="d-flex align-items-center">
                <small class="time text-secondary">{{ currentTime }}</small>
                <p :style="{ backgroundColor: message.color }">{{ message?.message }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ChatBox',
    props: {
        messages: Array,
        firstUser: Object,
        secondUser: Object
    },
    data: function () {
        return {}
    },
    computed: {
        currentTime() {
            let date = new Date();
            let time = date.toLocaleTimeString('es-CL', { hour: '2-digit', minute: '2-digit', hour12: false })
            return time
        }
    }
    ,
    //methods: {},
    // watch: {},
    // mixins: [],
    // filters: {},
    // -- Lifecycle Methods
    // -- End Lifecycle Methods
}
</script>

<style scoped>
.time {
    font-size: 10px
}
</style>