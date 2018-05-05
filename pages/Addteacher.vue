<template>
      <v-form v-model="valid">
        <div><h1>เพิ่มข้อมูลครู</h1></div>
  
  <v-text-field
      label="รหัสประจำตัว"
      v-model="name"
      :rules="nameRules"
      required
    ></v-text-field>
    <v-text-field
      label="ชื่อ-นามสกุล"
      v-model="name"
      :rules="nameRules"
      required
    ></v-text-field>
    <v-text-field
      label="E-mail"
      v-model="email"
      :rules="emailRules"
      required
    ></v-text-field>
  </v-form>
 
</template>
