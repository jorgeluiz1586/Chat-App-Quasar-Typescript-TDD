<template>
    <div style="width: 100wh; height: 100vh;">
        <div style="width: 100wh; height: 60vh; position: relative; background-color: blue;">
            <div style="display: none; width: 90px; height: 40px; padding: 2px; position: absolute; top: 0; right: 0;">
                <span>{{ timer }}</span>
            </div>
            <div style="width: 250px; height: 250px; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);">
                <img :src="user.photo" style="width: 250px; height: 250px; border-radius: 50%;"/>
            </div>
            <div style="width: 100%; height: 100px; position: absolute; bottom: 0; left: 0;">
                <h1 style="width: 100%; text-align: center; color: white; font-weight: bold; font-size: 2.4rem">{{ user.name }}</h1>
            </div>
        </div>
        <div style="width: 100wh; height: 40vh; position: relative; background-color: white;">
            <slot></slot>
        </div>
        <audio autoplay v-if="calling">
            <source src="../assets/audio/effects/calling.mp3" />
        </audio>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
    name: 'CallComponent',
    props: { 
        calling: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            user: { id: 2, photo: 'https://cdn.pixabay.com/photo/2016/06/19/22/46/girl-1467820_960_720.jpg', name: 'Camila', age: 23, status: 'Disponível', isOnline: true, message: 'Oi, tudo bem ?' },
            timer: new Date,
        };
    },
    setup() {
        1 + 1
    },
    // methods: {
    //     async listenToCall() {
    //         if (this.type === 'audio') {
    //             await navigator.mediaDevices.getUserMedia({ audio: {
    //                 sampleSize: 8,
    //                 echoCancellation: true
    //             }})
    //             .then(stream => {
    //                 let audioLocalComponent: HTMLAudioElement = document.createElement('audio');
    //                 audioLocalComponent.classList.add('audioComponent');
                    
    //                 audioLocalComponent.srcObject = stream 
    //                 void audioLocalComponent.play();
    //                 const socketIo = io('http://192.168.0.134:5000');

    //                 socketIo.on('test', (response: unknown) => {
    //                     console.log(response);
    //                 });

    //                 console.log('Got MediaStream:', stream);
    //             })
    //             .catch(error => {
    //                 console.error('Error accessing media devices.', error);
    //             });
    //         }

    //         if (this.type === 'video') {
    //             await navigator.mediaDevices.getUserMedia({video: true, audio: true})
    //             .then(stream => {
    //                 console.log('Got MediaStream:', stream);
    //             })
    //             .catch(error => {
    //                 console.error('Error accessing media devices.', error);
    //             });
    //         }
    //     },
    //     hangUpCall() {

    //     }
    // },
    // watch: {
    //     calling(value) {
    //         if(value === false) {
    //             void this.listenToCall()
    //         }
    //     }
    // },
    // mounted() {
    //     setTimeout(() => { this.calling = false}, 5000)
    // }
})
</script>

<style lang="scss" scoped>

</style>
