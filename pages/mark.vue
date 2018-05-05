<template>
    <div>
        <h1 class="info--text text-xs-center">แบบบันทึกคะแนนผู้เรียน</h1>
        <v-container grid-list-md text-xs-center>
            <v-form v-model="valid" ref="form" lazy-validation>  
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
                            v-model="groupId"
                            label="เลือกกลุ่มเรียน"
                            single-line
                        />
                    </v-flex>
                </v-layout>
            </v-form>
        </v-container>
    </div>
</template>


<script>
export default {
    data() {
        return {
            groupId: 0,
            teacherId: 0,
            subjectCode: '',
            studentId: 0,
            teacherList: [],
            subjectList: [],
            classList: [],
        }
    },
    computed: {
        teacherList2() {
            return this.teacherList.map(x => ({value: x.code, text: `${x.fname} ${x.lname}`}))
            
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
        this.getGroup()
    },
    methods: {
        async getGroup() {
            let res = await this.$http.get('/group')
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
        
    }
}
</script>
