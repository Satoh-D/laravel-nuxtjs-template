<template>
  <section class="section">
    <div class="columns">
      <div class="column">
        <video
          id="localvideo"
          ref="localVideo"
          autoplay
          :srcObject.prop="localstream"
          width="500"
          height="500"
        ></video>
      </div>
    </div>
    <div class="columns">
      <div class="column">
        <canvas
          id="localPic"
          ref="localPikCanvas"
          width="1280"
          height="700"
        ></canvas>
      </div>
    </div>
    <div class="buttons">
      <b-button @click="getUserMedia">映像を取得する</b-button>
      <b-button @click="getUserPic">写真を撮る</b-button>
      <!-- <b-button @click="getEmotion">表情判定</b-button> -->
    </div>
    <div class="columns">
      <div class="column"></div>
    </div>
  </section>
</template>

<script lang="ts">
/* eslint-disable no-console */
/*
import { Component, Vue } from 'vue-property-decorator'

@Component
export default class extends Vue {
  // dataはクラスのプロパティとして宣言する
  localstream: any
  canvas: any
  video: any
  photo: any
  streamConst = {
    video: {
      width: 1280,
      height: 700,
    },
  }

  // mounted
  mounted() {
    this.canvas = this.$refs.localPikCanvas
    this.video = this.$refs.localVideo
  }

  // methods
  getLocalMediaStream(mediaStream: any): void {
    this.localstream = mediaStream
  }

  handleLocalMediaStreamError(error: string): void {
    throw new Error(error)
    // console.error(`navigator.getUserMedia error: ${error}`)
  }

  getUserMedia(): void {
    navigator.mediaDevices
      .getUserMedia(this.streamConst)
      .then(this.getLocalMediaStream)
      .catch(this.handleLocalMediaStreamError)
  }
}
*/

export default {
  data() {
    return {
      localstream: undefined,
      canvas: undefined,
      video: undefined,
      photo: undefined,
      res: undefined,
      streamConst: {
        video: {
          width: 1280,
          height: 700,
        },
      },
    }
  },
  mounted() {
    this.canvas = this.$refs.localPikCanvas
    this.video = this.$refs.localVideo
  },
  computed: {},
  methods: {
    getLocalMediaStream(mediaStream: any) {
      console.log('getLocalmediaStream', mediaStream)
      this.localstream = mediaStream
    },
    handleLocalMediaStreamError(error: string) {
      console.error(`navigator.getUserMedia error: ${error}`)
    },
    getUserMedia() {
      console.log('getuserMedia')
      navigator.mediaDevices
        .getUserMedia(this.streamConst)
        .then(this.getLocalMediaStream)
        .catch(this.handleLocalMediaStreamError)
    },
    getUserPic() {
      const ctx = this.canvas.getContext('2d')

      // setInterval(function() {
      ctx.drawImage(this.movie, 0, 0, 640, 350)
      this.photo = this.canvas.toDataURL()
      // }, 500);
    },
    getEmotion() {
      const params = {
        image_data: this.photo,
      }
      this.res = this.$axios
        .$get('/face', { params })
        .then((res) => {
          console.log('resnpose data', res)
        })
        .catch((error) => {
          console.log('response error', error)
        })
    },
  },
}
</script>

<style></style>
