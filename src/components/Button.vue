<template>
    <button class="btn" :style="{ background: color }">
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
}

</script>
