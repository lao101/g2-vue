<template>
    <v-container grid-list-md text-xs-center>
        <v-layout row wrap>
            <v-flex xs8 offset-xs2>
                <v-card>
                    <v-card-text mt-5>
                        <h1>Chat room</h1>
                        <v-text-field v-model="name" label="ชื่อ" />
                        <v-text-field v-model="text" label="ข้อความ" />
                        <v-btn @click="send">ส่งข้อความ</v-btn>
                    </v-card-text>
                </v-card>
                <v-card>
                    <v-card-text mt-5>
                            <v-chip v-for='(m, idx) in msg' :key='idx' style="display: block; text-align: left">
                                <v-avatar class="teal">Msg</v-avatar>
                                <b>{{m.name}}</b> &nbsp;&nbsp;&gt; {{m.text}}
                            </v-chip>
                    </v-card-text>
                </v-card>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
export default {
    data() {
        return {
            name: '',
            text: '',
            msg: [],
        }
    }, //data

    // สำหรับ subscribe
    created() {
        // room39 คือ ห้องที่เราสามารถตั้งขั้นมาได้
        // this.onMsg คือ ชื่อเมธอดที่เราต้องสร้างขึ้น
        this.$socket.subscribe('room39', this.onMsg)
    }, //created

    // สำหรับ unsubscribe
    beforeDestroy() {
        this.$socket.unsubscribe('room39')
    }, //before Destory

    methods: {
        send() {
            this.$socket.publish('room39', {
                name: this.name,
                text: this.text,
            })
        }, // send()
        onMsg(data) {
            // this.msg.push(data) //ต่อท้าย
            this.msg.unshift(data)
            console.log(data)
        }, //onMsg
    }, //methods
}
</script>
