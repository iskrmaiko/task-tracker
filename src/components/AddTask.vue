<template>
    <form @submit="onSubmit" class="add-form">
        <div class="form-control">
            <label>Task</label>
            <input type="text" v-model="title" name="title" placeholder="Add Task" />
        </div>
        <div class="form-control">
            <label>Status</label>
            <input type="text" v-model="status" name="status" placeholder="Status" />
        </div>
        <div class="form-control form-control-check">
            <label>Set Reminder</label>
            <input type="checkbox" v-model="reminder" name="reminder" />
        </div>

        <input type="submit" value="Save Task" class="btn btn-block" />
    </form>
</template>

<script>
export default {
    name: 'AddTask',
    data() {
        return {
            title: '',
            status: '',
            reminder: false
        }
    },
    methods: {
        onSubmit(e) {
            e.preventDefault()
            if (!this.title) {
                alert('Please add a task')
                return
            }

            const newTask = {
                // id: Math.floor(Math.random() * 100000),
                title: this.title,
                status: this.status,
                reminder: this.reminder,
            }

            this.$emit('add-task', newTask)
            this.title = ''
            this.status = ''
            this.reminder = false
        }
    }
}
</script>

<style scoped>
.add-form {
    margin-bottom: 40px;
}

.form-control {
    margin: 20px 0;
}

.form-control label {
    display: block;
}

.form-control input {
    width: 100%;
    height: 40px;
    margin: 5px;
    padding: 3px 7px;
    font-size: 17px;
}

.form-control-check {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.form-control-check label {
    flex: 1;
}

.form-control-check input {
    flex: 2;
    height: 20px;
}
</style>