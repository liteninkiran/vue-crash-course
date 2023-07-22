<template>
    <header>
        <h1>{{ title }}</h1>
        <Button
            v-if="homePage"
            @btn-click="$emit('toggle-add-task')"
            :text="showAddTask ? 'Close' : 'Add Task'"
            :color="showAddTask ? 'red' : 'green'"
        />
    </header>
</template>

<script>

import Button from './Button';

export default {
    name: 'HeaderView',
    props: {
        title: {
            type: String,
            default: 'Hello World',
        },
        showAddTask: Boolean,
    },
    components: {
        Button,
    },
    computed: {
        homePage() {
            return this.$route.path === '/';
        }
    },
}

</script>

<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
</style>
