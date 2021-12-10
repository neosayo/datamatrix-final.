<template>
    <div id="activateWebcam" v-if="button == 'unused'">
        <button class="webcamOn" @click="this.button = 'clicked'">Cliquez pour activer la caméra</button>
    </div>
    <StreamBarcodeReader v-if="this.button == 'clicked' && text == ''"
        @decode="(a, b, c) => onDecode(a, b, c)"
        @loaded="() => onLoaded()"
    ></StreamBarcodeReader>
    <div v-if="this.button == 'used'" class="result">
        <div id="annonceResult"><b>Voici le résultat:</b> </div>
       <div id="responseText">
           <p>{{ text }}</p>
        </div>
    </div>
</template>

<script>

import { StreamBarcodeReader } from "vue-barcode-reader";

export default {
    name: "intendance",
    components: {
        StreamBarcodeReader,
    },
    data() {
        return {
            text: "",
            id: null,
            button:"unused"
        };
    },
    props: {
        msg: String,
    },
    methods:{
        onDecode(a, b, c) {
            this.button = 'used';
            this.text = a;
            var video = document.querySelector("#videoElement");
            var stream = video.srcObject;
            var tracks = stream.getTracks();
            for (let i = 0; i < tracks.length; i++){
                var track = tracks[i];
                track.stop();
            }
            video.srcObject = null;
        },
        onLoaded() {
            console.log("load");
        },
        stopWebcam(){
            this.button == "unused"
            var video = document.querySelector("video");
            var stream = video.srcObject;
            var tracks = stream.getTracks();

            for (let i = 0; i < tracks.length; i++){
                var track = tracks[i];
                track.stop();
            }
            video.srcObject = null;
        }
    }
};
</script>
<style>
</style>