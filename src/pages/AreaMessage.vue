<template>
    <div v-if="!calling">
        <header style="width: 100%; height: 60px; background-color: blue; display: flex; justify-content: space-around; align-items: center;">
            <div @click="$router.push('/')">
                <q-icon name="arrow_back" style="font-size: 30px; color: #fff; font-weight: bold"></q-icon>
            </div>
            <div style="width: 50px; height: 50px;">
                <img :src="user.photo" alt="" style="height: 50px; border-radius: 50%" />
            </div>
            <div style="width: 30%; height: 50px;">
                <h2 style="margin-top: -5%; font-size: 1.5rem; font-weight: bold; color: white">{{ user.name }}</h2>
            </div>
            <div style="width: 30%; height: 50px; display: flex; justify-content: space-between; align-items: center;">
                <div class="btn-call" @click="call('audio')">
                    <q-icon class="icon-call" name="call"></q-icon>
                </div>
                <div class="btn-call" @click="call('video')">
                    <q-icon class="icon-call" name="duo"></q-icon>
                </div>
            </div>
        </header>
        <h1>Teste</h1>
    </div>
    <CallComponent v-if="calling" :calling="callingSound">
        <button @click="hangUpCall()" style="width: 80px; height: 80px; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); border: none; border-radius: 50%; background-color: red; color: white;"><q-icon name="call" style="font-weight: bold; font-size: 45px"></q-icon></button>
    </CallComponent>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import CallComponent from '../components/CallComponent.vue'
import { io } from 'socket.io-client'

export default defineComponent({
    components: {
        CallComponent
    },
    data() {
        return {
            user: { id: 2, photo: 'https://cdn.pixabay.com/photo/2016/06/19/22/46/girl-1467820_960_720.jpg', name: 'Camila', age: 23, status: 'Disponível', isOnline: true, message: 'Oi, tudo bem ?' }, 
            calling: false,
            callingSound: true,
            type: '',
            audioLocal: '',
            videoLocal: ''
        };
    },
    methods: {
        call(type: string): void
        {
            this.type = type;
            this.calling = true;
            setTimeout(() => { this.callingSound = false}, 5000)
        },
        async listenToCall() {
            if (this.type === 'audio') {
                await navigator.mediaDevices.getUserMedia({ audio: {
                    sampleSize: 8,
                    echoCancellation: true
                }})
                .then(stream => {
                    let audioLocalComponent: HTMLAudioElement = document.createElement('audio');
                    audioLocalComponent.classList.add('audioComponent');
                    
                    audioLocalComponent.srcObject = stream 
                    void audioLocalComponent.play();
                    const socketIo = io('http://192.168.0.134:5000');

                    socketIo.on('test', (response: unknown) => {
                        console.log(response);
                    });

                    console.log('Got MediaStream:', stream);
                })
                .catch(error => {
                    console.error('Error accessing media devices.', error);
                });
            }

            if (this.type === 'video') {
                await navigator.mediaDevices.getUserMedia({video: true, audio: true})
                .then(stream => {
                    console.log('Got MediaStream:', stream);
                })
                .catch(error => {
                    console.error('Error accessing media devices.', error);
                });
            }
        },
        hangUpCall() {
            this.calling = false;
        }
    },
    setup() {
        1 + 1;
    },
    watch: {
        callingSound(value) {
            if(value === false) {
                void this.listenToCall()
            }
        }
    }
})
</script>

<style lang="scss" scoped>
.btn-call
{
    width: 45%;
    height: initial;
    .icon-call
    {
        font-size: 30px;
        font-weight: bold;
        color: #fff;
    }
}
</style>
