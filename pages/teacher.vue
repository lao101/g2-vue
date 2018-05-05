<template>
    <div>
        <div class="text-xs-right">
            <v-btn color="primary" dark @click.stop="dialogAdd = !dialogAdd"><v-icon>add</v-icon> Add New</v-btn>
        </div>
        <v-data-table :headers="headers" :items="teachers" class="elevation-3">
            <template slot="items" slot-scope="props">
                <td class="text-xs-left">{{ props.item.code }}</td>
                <td class="text-xs-left">{{ props.item.fname }}</td>
                <td class="text-xs-left">{{ props.item.lname }}</td>
                <td class="text-xs-left"><v-icon @click="editTeacher(props)" class="green--text" style="cursor: pointer">edit</v-icon></td>
                <td class="text-xs-left"><v-icon @click="deleteTeacher(props.item.id)" class="red--text" style="cursor: pointer">delete</v-icon></td>
                <td class="text-xs-left"><v-icon @click="detailTeacher(props)" class="info--text" style="cursor: pointer">dvr</v-icon></td>
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
                        label="รหัสครูผู้สอน"
                        v-model="tcode"
                        :rules="codeRules"
                        :counter="5"
                        required
                    ></v-text-field>
                    </v-flex>
                    <v-flex md12>
                        <v-text-field
                            label="ชื่อ"
                            v-model="tfname"
                            :rules="fnameRules"
                            :counter="50"
                            required
                        />   
                    </v-flex>
                    <v-flex md12>
                        <v-text-field
                            label="นามสกุล"
                            v-model="tlname"
                            :rules="lnameRules"
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

        <!-- v-dialog detailTeacher -->
        <v-dialog v-model="dialogDetail" max-width="500px">
            <v-card>
                <v-card-text>
                    <v-container grid-list-md text-xs-center>
                        <v-layout row wrap> 
                            <v-flex md12>ID: {{dtid}}</v-flex>
                            <v-flex md12>Code: {{dtcode}}</v-flex>
                            <v-flex md12>First Name: {{dtfname}}</v-flex>
                            <v-flex md12>Last Name: {{dtlname}}</v-flex>
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
                            label="รหัสครูผู้สอน"
                            v-model="edcode"
                            required
                            readonly
                        />
                    </v-flex>
                    <v-flex md12>
                        <v-text-field
                            label="ชื่อ"
                            v-model="edfname"
                            required
                        />   
                    </v-flex>
                    <v-flex md12>
                        <v-text-field
                            label="นามสกุล"
                            v-model="edlname"
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
            teachers: [],
            headers: [
                { text: 'Code', align: 'left', sortable: false},
                { text: 'First Name', align: 'left', sortable: false},
                { text: 'Last Name', align: 'left', sortable: false},
                { text: 'Update', align: 'left', sortable: false},
                { text: 'Delete', align: 'left', sortable: false},
                { text: 'Detial', align: 'left', sortable: false},
            ],

            //addTeacher Data
            dialogAdd: false,
            tcode: '',
            codeRules: [
                v => !!v || 'Code is required',
                v => (v && v.length <= 4) || 'รหัสครูผู้สอนต้องมี 4 หลัก'
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

            //detailTeacher Data
            dialogDetail: false,
            dtid: '',
            dtcode: '',
            dtfname: '',
            dtlname: '',

            //editTeacher Data
            dialogEdit: false,
            edid: '',
            edcode: '',
            edfname: '',
            edlname: '',
        }
    },
  
    // ไว้ มอนิเตอร์ ตัวแปร เช่่น หากตัวแปร cls เปลี่ยนค่า จะเรียก getStudent() 
    watch: {
        cls() {
            this.getTeacher()
        }
    },

    // สำหรับเรียกใช้งานครั้งแรก
    async created() {
        this.getTeacher()
    }, // created

    methods: {
            async getTeacher() {
                let res = await this.$http.get('/teacher')
                this.teachers = res.data.teachers
                console.log(this.teachers)
            },

            editTeacher(prop) {
                this.dialogEdit = !this.dialogEdit
                this.edid = prop.item.id,
                this.edcode = prop.item.code,
                this.edfname = prop.item.fname,
                this.edlname = prop.item.lname
            },

            detailTeacher(dt) {
                this.dialogDetail = !this.dialogDetail
                this.dtid = dt.item.id
                this.dtcode = dt.item.code
                this.dtfname = dt.item.fname
                this.dtlname = dt.item.lname
            },

            deleteTeacher(id) {
                let res = this.$http.delete('/teacher/delete/' + id)
                this.getTeacher()
            },
            
            submit() {
            if (this.$refs.form.validate()) {
                this.$http.post('/teacher/add', {
                    code: this.tcode,
                    fname: this.tfname,
                    lname: this.tlname,
                }).then((res)=>{
                    if (res.data.status === true) {
                        this.dialogAdd = !this.dialogAdd
                    }
                })
                this.getTeacher()
                // alert(res.data.status)
            }},

            clear() {
                this.$refs.form.reset()
            },

            async submitEdit(id) {
                let res = await this.$http.post('/teacher/save/' + id, {
                    fname: this.edfname,
                    lname: this.edlname,
                })
                if (res.data.status === true)
                    this.dialogEdit = !this.dialogEdit
                this.getTeacher()
            }
    }, //method
}
</script>
