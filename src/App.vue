<script setup>
  import Vplayer from "./components/VideoPlayer.vue";
</script>

<template>
<Vplayer :isLocal="true" :stream="lclStrm"/>
<Vplayer :isLocal="false" :stream="rmtStrm"/>
</template>

<script>
async function createOffer(){
    var pc = new RTCPeerConnection({
        iceServers: [
            {
                urls: ['stun:stun.l.google.com:19302', 'stun:stun4.l.google.com:19302']
            }
        ]
    })
    var offer = await pc.createOffer()
    await pc.setLocalDescription(offer)

    console.log("Offer", {text:JSON.stringify({'type': 'offer', 'offer': offer})})
    return pc
}

export default {
  data() {
    return {
      lclStrm: null,
      rmtStrm: null,
      peerCx: null,
      offer: null,
      roomID: "",
    }
  },
  methods: {
    setTracks(){
      this.lclStrm.getTracks().forEach((track) => {
        console.log(track)
        this.peerCx.addTrack(track, this.lclStrm)
      })

      this.peerCx.ontrack = (event) => {
        event.streams[0].getTracks().forEach((track) => {
          this.rmtStrm.addTrack(track)
          })
      }

      this.peerCx.onicecandidate = async (event) => {
        if (event.candidate) {
          console.log("ICECandidate", {text:JSON.stringify({'type':'candidate', 'candidate': event.candidate})})
        }
      }
    },
  },
  async created() {
    this.peerCx = await createOffer()
    this.lclStrm = await navigator.mediaDevices.getUserMedia({video: true, audio: false})
    this.rmtStrm = new MediaStream()
    this.setTracks()
  }
}
</script>

<style>
</style>
