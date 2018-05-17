<template>
    <div>
        <h1 class="info--text text-xs-center">แบบบันทึกคะแนนผู้เรียน</h1>
        <v-container grid-list-md text-xs-center>
            <v-layout row wrap>
                <v-flex xs6 offset-xs3>
                    <v-select
                        :items="teacherList2"
                        v-model="teacherId"
                        label="เลือกครู"
                        single-line
                    />
                </v-flex>
            </v-layout>
            <v-layout row wrap>
                <v-flex xs6 offset-xs3>
                    <v-select
                        :items="subjectList"
                        item-value="subjCode"
                        item-text="subjDescription"
                        v-model="subjectId"
                        label="เลือกวิชา"
                        single-line
                    />
                </v-flex>
            </v-layout>
            <v-layout row wrap>
                <v-flex xs6 offset-xs3>
                    <v-select
                        :items="classList"
                        item-value="classCode"
                        item-text="classDescript"
                        v-model="classId"
                        label="เลือกกลุ่มเรียน"
                        single-line
                    />
                </v-flex>
            </v-layout>
            <v-layout row wrap>
                <v-flex xs6 offset-xs3>
                    <v-btn @click="getStudent"><v-icon class="success--text">repeat</v-icon> GET Student!!</v-btn>
                </v-flex>
            </v-layout>
        </v-container>

        <v-container v-if="seen">
             <v-layout row wrap>
                <v-flex xs8 offset-xs2>
                    <v-data-table :headers="headers" :items="students" hide-actions class="elevation-1">
                        <template slot="items" slot-scope="props">
                            <td class="text-xs-left">{{ props.item.id }}</td>
                            <td class="text-xs-left">{{ props.item.code }}</td>
                            <td class="text-xs-left">{{ props.item.fname }}</td>
                            <td class="text-xs-left">{{ props.item.lname }}</td>
                            <td class="text-xs-left">
                                <v-text-field label="คะแนน" v-model="props.item.mark" required/>
                            </td>
                        </template>
                    </v-data-table>
                </v-flex>
                <v-flex xs8 offset-xs2>
                    <v-btn class="success" @click="saveMark">SAVE !!</v-btn>
                </v-flex>
            </v-layout>           
        </v-container>

        <v-dialog v-model="dialogMessage" max-width="500px">
            <v-card>
                <v-card-text>
                    <v-container grid-list-md text-xs-center>
                        <h1>ยังทำไม่สำเร็จครับ !!</h1>
                        <v-layout row wrap> 
                            <v-flex md12>Teacher ID: {{teacherId}}</v-flex>
                            <v-flex md12>Subject ID: {{subjectId}}</v-flex>
                            <v-flex md12>Classroom ID: {{classId}}</v-flex>
                        </v-layout>
                    </v-container>
                </v-card-text>
                <v-card-actions>
                    <v-btn color="primary" flat @click.stop="dialogMessage=false">Close</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

    </div>
</template>


<script>
export default {
    data() {
        return {
            teacherId: 0,
            subjectId: '',
            classId: '',
            studentId: '',
            teacherList: [],
            subjectList: [],
            classList: [],
            students: [],
            seen: false,
            headers: [
                { text: 'ID', align: 'left', sortable: false},
                { text: 'Code', align: 'left', sortable: false},
                { text: 'First Name', align: 'left', sortable: false},
                { text: 'Last Name', align: 'left', sortable: false},
                { text: 'Mark', align: 'left', sortable: false},
            ],
            dialogMessage: false,
        }
    },
    computed: {
        teacherList2() {
            return this.teacherList.map(x => ({value: x.code, text: `${x.fname} ${x.lname}`}))
            
            //แบบเก่า
            // let out = []
            // for (let i = 0; i < this.teacherList.length; i++) {
            //     out.push({
            //         value: this.teacherList[i].code,
            //         text: this.teacherList[i].fname + ' ' + this.teacherList[i].lname,
            //     })
            // }
            // return out
        }
    },
    created() {
        this.getTeacher()
        this.getSubject()
        this.getClassroom()
    },
    methods: {
        async getClassroom() {
            let res = await this.$http.get('/classroom')
            this.classList = res.data.classrooms
        },

        async getTeacher() {
            let res = await this.$http.get('/teacher')
            console.log(res.data.teachers)
            this.teacherList = res.data.teachers
        },

        async getSubject() {
            let res = await this.$http.get('/subject')
            console.log(res.data.subjects)
            this.subjectList = res.data.subjects
        },

        async getStudent() {
            let res = await this.$http.get('/student/classroom/' + this.classId)
            if (res.data.students.length > 0) {
                this.students = res.data.students
                // alert(this.students[0].id)
                this.seen = true
            } else {
                // alert("NO")
                this.seen = false
            }
            console.log(this.students)
        },

        saveMark() {
            console.log({
                teacherId: this.teacherId,
                subid: this.subjectId,
                marks: this.students.map(st => ({sid: st.code, mark: st.mark})),
            })
            this.$http.post('/mark/save', {
                teacherId: this.teacherId,
                subid: this.subjectId,
                marks: this.students.map(st => ({sid: st.code, mark: st.mark, grade: this.setMark(st.mark)})),
            })
            console.log(this.students)
            // this.dialogMessage = !this.dialogMessage
        },

        setMark(mark) {
            let grade = 0
            if (mark<50) {
                grade = 0
            } else if(mark<55) {
                grade = 1
            } else if(mark<60) {
                grade = 1.5
            } else if(mark<65) {
                grade = 2
            } else if(mark<70) {
                grade = 2.5
            } else if(mark<75) {
                grade = 3
            } else if(mark<80) {
                grade = 3.5
            } else {
                grade = 4
            }
            return grade
        },
        
    }
}
</script>
