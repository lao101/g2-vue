<template>
    <div>
        <div class="text-xs-right">
            <v-btn color="primary" dark><v-icon>add</v-icon> Add New</v-btn>
        </div>
        <v-data-table :headers="headers" :items="subjects" class="elevation-3">
            <template slot="items" slot-scope="props">
                <td class="text-xs-left">{{ props.item.subjCode }}</td>
                <td class="text-xs-left">{{ props.item.subjDescription }}</td>
                <td class="text-xs-left">{{ props.item.subjCredit }}</td>
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
            subjects: [],
            headers: [
                { text: 'Subject Code', align: 'left', sortable: false},
                { text: 'Subject', align: 'left', sortable: false},
                { text: 'Subject Credit', align: 'left', sortable: false},
                { text: 'Update', align: 'left', sortable: false},
                { text: 'Delete', align: 'left', sortable: false},
                { text: 'Detial', align: 'left', sortable: false},
            ],
        }
    },
    
    created() {
        this.getSubject()
    },

    methods: {
            async getSubject() {
                let res = await this.$http.get('/subject')
                this.subjects = res.data.subjects
                console.log(this.subjects)
            },
    }, //method
}
</script>
