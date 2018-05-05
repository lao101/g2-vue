<template>
    <div>
        <div class="text-xs-right">
            <v-btn color="primary" dark @click.stop="dialogAdd = !dialogAdd"><v-icon>add</v-icon> Add New</v-btn>
        </div>
        <v-data-table
            :headers="headers"
            :items="classrooms"
            hide-actions
            class="elevation-1">
                <template slot="items" slot-scope="props">
                    <td class="text-xs-left">{{ props.item.classCode }}</td>
                    <td class="text-xs-left">{{ props.item.classDescript }}</td>
                    <td class="text-xs-left"><v-icon @click="editclassroom(props)" class="green--text" style="cursor: pointer">mode edit</v-icon></td>
                    <td class="text-xs-left"><v-icon @click="deleteclassroom(props.item.id)" class="red--text" style="cursor: pointer">delete forever</v-icon></td>
                    <td class="text-xs-left"><v-icon @click="detailclassroom(props)" class="info--text" style="cursor: pointer">dvr</v-icon></td>
                </template>
        </v-data-table>

        <v-dialog v-model="dialogAdd" max-width="500px">
            <v-card>
            <v-card-text>

                <v-container grid-list-md text-xs-center>
                <v-form v-model="valid" ref="form" lazy-validation>  
                <v-layout row wrap>
                    <v-flex md12>
                    <v-text-field
                        label="รหัสกลุ่ม"
                        v-model="tcode"
                        :rules="codeRules"
                        :counter="5"
                        required
                    ></v-text-field>
                    </v-flex>
                    <v-flex md12>
                        <v-text-field
                            label="ชื่อกลุ่ม"
                            v-model="tfname"
                            :rules="fnameRules"
                            :counter="50"
                            required
                        />   
                    </v-flex>
                    <v-flex md6 offset-md3>
                        <v-btn
                            @click="submit"
                            :disabled="!valid"
                            >
                            บันทึกข้อมูล
                        </v-btn>
                        <v-btn @click="clear">ล้าง</v-btn>
                    </v-flex>  
                </v-layout>
                </v-form> 
            </v-container>

            </v-card-text>
            <v-card-actions>
                <v-btn color="primary" flat @click.stop="dialogAdd=false">Close</v-btn>
            </v-card-actions>
            </v-card>
        </v-dialog> <!-- v-dialog Add -->

        <!-- v-dialog detailclassroom -->
        <v-dialog v-model="dialogDetail" max-width="500px">
            <v-card>
            <v-card-text>
                <v-container grid-list-md text-xs-center>
                    <v-layout row wrap> 
                        <v-flex md12>ID: {{dtid}}</v-flex>
                        <v-flex md12>Code: {{dtcode}}</v-flex>
                        <v-flex md12>First Name: {{dtfname}}</v-flex>
                    </v-layout>
                </v-container>
            </v-card-text>
            <v-card-actions>
                <v-btn color="primary" flat @click.stop="dialogDetail=false">Close</v-btn>
            </v-card-actions>
            </v-card>
        </v-dialog> <!-- v-dialog Detail -->

        <v-dialog v-model="dialogEdit" max-width="500px">
            <v-card>
            <v-card-text>

                <v-container grid-list-md text-xs-center>
                <v-form v-model="valid" ref="form" lazy-validation>  
                <v-layout row wrap>
                    <v-flex md12>
                        <v-text-field
                            label="ID"
                            v-model="edid"
                            readonly
                        />
                    </v-flex>
                    <v-flex md12>
                        <v-text-field
                            label="รหัสกลุ่ม"
                            v-model="edcode"
                            required
                            readonly
                        />
                    </v-flex>
                    <v-flex md12>
                        <v-text-field
                            label="ชื่อกลุ่ม"
                            v-model="edfname"
                            required
                        />   
                    </v-flex>
                    <v-flex md6 offset-md3>
                        <v-btn
                            @click="submitEdit(edid)"
                            :disabled="!validEdit"
                            >
                            บันทึกข้อมูล
                        </v-btn>
                    </v-flex>  
                </v-layout>
                </v-form> 
            </v-container>

            </v-card-text>
            <v-card-actions>
                <v-btn color="primary" flat @click.stop="dialogEdit=false">Close</v-btn>
            </v-card-actions>
            </v-card>
        </v-dialog> <!-- v-dialog Edit -->        
    </div>   
</template>


<script>
export default {
    data() {
        return {
            cls: '1',
            classrooms: [],
            headers: [
                { text: 'Code', align: 'left', sortable: false},
                { text: 'Group Name', align: 'left', sortable: false},
                { text: 'Update', align: 'center', sortable: false},
                { text: 'Delete', align: 'left', sortable: false},
                { text: 'Detial', align: 'left', sortable: false},
            ],

            //addclassroom Data
            dialogAdd: false,
            tcode: '',
            codeRules: [
                v => !!v || 'Code is required',
                v => (v && v.length <= 4) || 'รหัสกลุ่มต้องมี 4 หลัก'
            ],
            tfname: '',
            fnameRules: [
                v => !!v || 'First Name is required',
            ],
            tlname: '',
            lnameRules: [
                v => !!v || 'Last Name is required',
            ],
            valid: true,
            validEdit: true,

            //detailclassroom Data
            dialogDetail: false,
            dtid: '',
            dtcode: '',
            dtfname: '',

            //editclassroom Data
            dialogEdit: false,
            edid: '',
            edcode: '',
            edfname: '',
        }
    },
  
    // ไว้ มอนิเตอร์ ตัวแปร เช่่น หากตัวแปร cls เปลี่ยนค่า จะเรียก getStudent() 
    watch: {
        cls() {
            this.getclassroom()
        }
    },

    // สำหรับเรียกใช้งานครั้งแรก
    async created() {
        this.getclassroom()
    }, // created

    methods: {
            async getclassroom() {
                let res = await this.$http.get('/classroom')
                this.classrooms = res.data.classroom
                // console.log(res.data.status)
            },

            editclassroom(prop) {
                this.dialogEdit = !this.dialogEdit
                this.edid = prop.item.id,
                this.edcode = prop.item.classCode,
                this.edfname = prop.item.classDescript
            },

            detailclassroom(dt) {
                this.dialogDetail = !this.dialogDetail
                this.dtid = dt.item.id
                this.dtcode = dt.item.classCode
                this.dtfname = dt.item.classDescript
            },

            deleteclassroom(id) {
                let res = this.$http.delete('/classroom/delete/' + id)
                this.getclassroom()
            },
            
            submit() {
            if (this.$refs.form.validate()) {
                this.$http.post('/classroom/add', {
                    classCode: this.tcode,
                    classDescript: this.tfname,
                }).then((res)=>{
                    if (res.data.status === true) {
                        this.dialogAdd = !this.dialogAdd
                    }
                    this.getclassroom()
                })
                // alert(res.data.status)
            }},

            clear() {
                this.$refs.form.reset()
            },

            async submitEdit(id) {
                // alert("submitEdit" + id)
                let res = await this.$http.post('/classroom/save/' + id, {
                    classDescript: this.edfname,
                })
                if (res.data.status === true)
                    this.dialogEdit = !this.dialogEdit
                this.getclassroom()
            }
    }, //method
}
</script>
