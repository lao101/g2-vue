<template>
  <v-layout row>
    <v-flex xs12 sm6 offset-sm3>
      <v-card>
        <v-toolbar color="cyan" dark>
          <v-toolbar-title>ระบบบันทึกคะแนนผู้เรียน</v-toolbar-title>
          <v-spacer></v-spacer>
        </v-toolbar>
        <v-list two-line>
          <template v-for="(item, index) in items">
            <v-subheader v-if="item.header" :key="item.header">{{ item.header }}</v-subheader>
            <v-divider v-else-if="item.divider" :inset="item.inset" :key="index"></v-divider>
            <v-list-tile v-else :key="item.title" avatar>
              <v-list-tile-avatar>
                <img :src="item.avatar">
              </v-list-tile-avatar>
              <v-list-tile-content>
                <v-list-tile-title v-html="item.title"></v-list-tile-title>
                <v-list-tile-sub-title v-html="item.subtitle"></v-list-tile-sub-title>
              </v-list-tile-content>
            </v-list-tile>
          </template>
        </v-list>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
  export default {
    data () {
      return {
        items: [
          { header: 'รายชื่อผู้พัฒนา' },
          { avatar: '/images/1.jpg', title: 'นายนพอนันต์ นรโคตร', subtitle: "วอศ.สุรินทร์" },
          { divider: true, inset: true },
          { avatar: '/images/2.jpg', title: 'นายสุระชัย วิเศษโวหาร', subtitle: "วอศ.ร้อยเอ็ด" },
          { divider: true, inset: true },
          { avatar: '/images/3.jpg', title: 'นางสาวสุนันท์นุช  มีแสง', subtitle: "วท.ชลบุรี" }
        ]
      }
    }
  }
</script>