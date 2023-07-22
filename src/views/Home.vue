<template>
    <div v-if="showAddTask">
        <AddTask @add-task="addTask" />
    </div>
    <Tasks
        @toggle-reminder="toggleReminder"
        @delete-task="deleteTask"
        :tasks="tasks"
    />
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
    name: 'HomeView',
    components: {
            Tasks,
            AddTask,
        },
    data() {
        return {
            tasks: [],
        };
    },
    props: {
        showAddTask: Boolean,
    },
    async created() {
        this.tasks = await this.fetchTasks();
    },
    methods: {
        async deleteTask(id) {
            if (confirm('Are you sure?')) {
                const res = await fetch(`api/tasks/${id}`, {
                    method: 'DELETE',
                });
                res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error deleting task') ;
            }
        },
        async toggleReminder(id) {
            const taskToToggle = await this.fetchTask(id);
            const updateTask = {
                ...taskToToggle,
                reminder: !taskToToggle.reminder,
            };
            const res = await fetch(`api/tasks/${id}`, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(updateTask),
            });
            const data = await res.json();
            this.tasks = this.tasks.map(
                (task) => {
                    const newTask = { ...task, reminder: data.reminder };
                    return task.id === id ? newTask : task;
                }
            );
        },
        async addTask(task) {
            const res = await fetch('api/tasks', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify(task),
            });
            const data = await res.json();
            this.tasks = [...this.tasks, data];
        },
        async fetchTasks() {
            const res = await fetch('api/tasks');
            return await res.json();
        },
        async fetchTask(id) {
            const res = await fetch(`api/tasks/${id}`);
            return await res.json();
        },
    },
};
</script>
