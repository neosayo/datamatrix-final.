<template>
        <uses-permission android:name="android.permission.CAMERA" />
        <uses-feature android:name="android.hardware.camera" />
    <body>
        <div id="container">
            <video autoplay="true" id="videoElement"></video>
        </div>
        <div>
            <button id="stopButton" @click=disableWebcam()>Stoppez tout</button>
        </div>
    </body>
</template>

<script>
export default {
    methods:{
        activateWebcam(){
            
            var video = document.querySelector("#videoElement");

            if (navigator.mediaDevices.getUserMedia){
                navigator.mediaDevices.getUserMedia({video: true})
                .then(function (stream){
                    video.srcObject = stream;
                })
                .catch(function(error){
                    console.log(error);
                })
            }
        },
        disableWebcam(e){
            var video = document.querySelector("#videoElement");
            var stream = video.srcObject;
            var tracks = stream.getTracks();

            for (let i = 0; i < tracks.length; i++){
                var track = tracks[i];
                track.stop();
            }
            video.srcObject = null;
            navigator.mediaDevices.getUserMedia({video: false})
        }
    }
}
</script>



<style>

</style>
