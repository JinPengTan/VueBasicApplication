<template>
    <div id="employee-table">
        <p v-if="employees.length < 1" class="empty-table">
            No Employees
        </p>
        <table v-else>
            <thead>
                <tr>
                    <th>Employee name</th>
                    <th colspan="2">Employee email</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in employees" :key="employee.id">
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.name">
                    </td>
                    <td v-else>{{ employee.name }}</td>
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.email">
                    </td>
                    <td v-else>{{ employee.email }}</td>
                    <td v-if="editing === employee.id">
                        <button @click="editEmployee(employee)"> Save </button>
                        <button class="muted-button" @click="editing = null"> Cancel </button>
                    </td>
                    <td v-else>
                        <button v-on:click="editMode(employee.id)">Edit</button>
                        <button v-on:click="$emit('delete:employee', employee.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
            <p v-if="success" class="success-message">
                ✅ Employee successfully added
            </p>
        </table>
    </div>
</template>

<script>
    export default {
        name: "EmployeeTable",
        props: {
            employees: Array,
            success: Boolean,
        },
        data() {
            return {
                editing: null,
            }
        },
        methods: {
            editMode(id) {
                this.editing = id
            },
            editEmployee(employee) {
                if(employee.name === '' || employee.email === '') return
                this.$emit('edit:employee', employee.id, employee)
                this.editing = null
            }
        }
    }
</script>

<style scoped>
    button {
        margin: 0 0.5rem 0 0;
    }

    .error-message {
        color: #d33c40;
    }

    .success-message {
        color: #32a95d;
    }
</style>