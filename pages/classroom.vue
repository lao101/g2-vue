<template>
    <div>
        <div class="text-xs-right">
            <v-btn color="primary" dark><v-icon>add</v-icon> Add New</v-btn>
        </div>
        <v-data-table :headers="headers" :items="classrooms" class="elevation-3">
            <template slot="items" slot-scope="props">
                <td class="text-xs-left">{{ props.item.id }}</td>
                <td class="text-xs-left">{{ props.item.classCode }}</td>
                <td class="text-xs-left">{{ props.item.classDescript }}</td>
                <td class="text-xs-left"><v-icon class="green--text" style="cursor: pointer">edit</v-icon></td>
                <td class="text-xs-left"><v-icon class="red--text" style="cursor: pointer">delete</v-icon></td>
                <td class="text-xs-left"><v-icon class="info--text" style="cursor: pointer">dvr</v-icon></td>
            </template>
        </v-data-table>   
    </div>   
</template>


<script>
export default {
    data() {
        return {
            cls: '1',
            classrooms: [],
            headers: [
                { text: 'ClassRoom ID', align: 'left', sortable: false},
                { text: 'ClassRoom Code', align: 'left', sortable: false},
                { text: 'ClassRoom Description', align: 'left', sortable: false},
                { text: 'Update', align: 'left', sortable: false},
                { text: 'Delete', align: 'left', sortable: false},
                { text: 'Detial', align: 'left', sortable: false},
            ],
        }
    },
    
    created() {
        this.getClass()
    },

    methods: {
            async getClass() {
                let res = await this.$http.get('/classroom')
                this.classrooms = res.data.classrooms
                console.log(this.classrooms)
            },
    }, //method
}
</script>
