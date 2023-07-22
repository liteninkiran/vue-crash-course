<template>
    <button class="btn" :style="{ background: color }" @click="onClick()">
        {{ text }}
    </button>
</template>

<script>

export default {
    name: 'ButtonView',
    props: {
        text: String,
        color: { type: String, default: 'grey' },
    },
    methods: {
        onClick() {
            this.$emit('btn-click');
        }
    },
}

</script>
