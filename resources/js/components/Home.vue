<template>
    <div class="container mt-3">
        <div class="row">
            <div class="col-md-6 mx-auto">
                <div class="card">
                    <div class="card-header">
                        <h1 class="text-center">Todo App</h1>
                    </div>

                    <div class="card-body">
                        <form v-on:submit.prevent="addNewTask">
                            <label for="">Task name:</label>
                            <input v-model="title" type="text" class="form-control" placeholder="Add new task...">
                            <button v-if="this.isEdit == false" type="submit" class="btn btn-block btn-success mt-3">Submit</button>
                            <button v-else type="button" v-on:click="updateTask" class="btn btn-block btn-primary mt-3">Update</button>
                        </form>

                        <table class="table">
                            <tr v-for="task in tasks" v-bind:key="task.id">
                                <td class="text-left">{{ task.title }}</td>
                                <td class="text-right">
                                    <button v-on:click="editTask(task.title, task.id)" class="btn btn-primary">Edit</button>
                                    <button v-on:click="deleteTask(task.id)" class="btn btn-danger">Delete</button>
                                </td>
                            </tr>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            tasks: [],
            id: '',
            title: '',
            isEdit: false
        }
    },
    mounted () {
        this.getTasks ()
    },
    methods: {
        getTasks () {
            axios.get('/api/task')
            .then((result) => {
                this.tasks = result.data
            }).catch((err) => {
                console.log(err)
            })
        },
        addNewTask () {
            axios.post('/api/task',
            { title: this.title }
            ).then((result) => {
                this.title = ''
                this.getTasks()
            }).catch((err) => {
                console.log(err)
            })
        },
        editTask (title, id) {
            this.id = id
            this.title = title
            this.isEdit = true
        },
        updateTask () {
            axios.put(`/api/task/${this.id}`,
            { title: this.title }
            ).then((result) => {
                this.title = ''
                this.isEdit = false
                this.getTasks()
            }).catch((err) => {
                console.log(err)
            })
        },
        deleteTask (id) {
            axios.delete(`/api/task/${id}`
            ).then((result) => {
                this.title = ''
                this.getTasks()
            }).catch((err) => {
                console.log(err)
            })
        }
    }
}
</script>

<style>

</style>
