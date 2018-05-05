<template>
  <div>
    <h2 style="color:blue">บันทึกข้อมูลครูผู้สอน</h2>
    <v-container grid-list-md text-xs-center>
    <v-form v-model="valid" ref="form" lazy-validation>  
      <v-layout row wrap>
        <v-flex md3>
          <v-text-field
            label="รหัสครูผู้สอน"
            v-model="teachId"
            :rules="idRules"
            :counter="13"
            required
          ></v-text-field>
        </v-flex>
        <v-flex md6>
          <v-text-field
            label="ชื่อ-สกุล"
            v-model="teachName"
            :rules="nameRules"
            :counter="40"
            required
          ></v-text-field>   
        </v-flex>
        <v-flex md3>
          <v-text-field
            label="E-mail"
            v-model="email"
            :rules="emailRules"
            required
          ></v-text-field>   
        </v-flex>
        <v-flex md6>
          <v-select
            label="แผนกวิชา"
            v-model="departCode"
            :items="items"
            required
          ></v-select>
        </v-flex>
        <v-flex md6>
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
  </div>
</template>
<script>
  import axios from 'axios'

  export default {
    data: () => ({
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 10) || 'Name must be less than 10 characters'
      ],
      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
      ],
      select: null,
      items: [
        'เทคโนโลยีสารสนเทศ',
        'คอมพิวเตอร์ธุรกิจ',
        'บัญชี',
        'การจัดการสำนักงาน'
      ],
      checkbox: false
    }),

    methods: {
      submit () {
        if (this.$refs.form.validate()) {
          // Native form submission is not yet supported
          axios.post('/api/submit', {
            name: this.name,
            email: this.email,
            select: this.select,
            checkbox: this.checkbox
          })
        }
      },
      clear () {
        this.$refs.form.reset()
      }
    }
  }
</script>
