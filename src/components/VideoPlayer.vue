<script setup>
import test from '@/components/webrtc'
</script>

<template>
    <h3 class="test">{{playerText}}</h3>
    <div class="videoDiv">
        <video class="videoPlayer" muted controls autoplay :srcObject=stream></video>
    </div>

</template>
<script>



export default {
    data(){
        return {
            toto: null
        }
    },
    props: ['isLocal', 'stream'],
    computed: {
        playerText() {
            if (this.isLocal){
                return "Your Stream"
            }
            else{
                return "Your Opponent"
            }
        }
    },
    async created() {

    }
}
</script>

<style>
.videoDiv {
    height: 240px;
    width: 352px;
    background-color: gray;
}

.videoPlayer {
    width:80%;
    height:80%;
    background-color: blue;
    display: block;
    margin-top: auto;
    margin-left: auto;
    margin-right: auto;
}

</style>
