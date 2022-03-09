<template>
  <div class="audioReading">
    <audio :src="audioUrl" controls="true">暂不支持audio媒体播放</audio>
  </div>
</template>
<script>
  import {audioReading} from "../../axios/api";
export default {
  name: 'audioReading',
  props: {
    // 文章内容
    cpContent: {
      type: String,
      default: ''
    }
  },
  watch: {
    cpContent: {
      handler(val) {
        if(val) {
          this.getMediaSrc();
        }
      }
    }
  },
  data() {
    return {
      sliceLen: 1000,
      audioUrl: '',
    }
  },
  methods: {
    getMediaSrc() {
      const len = this.cpContent.length > this.sliceLen ? this.sliceLen : this.cpContent.length;
      const txt = this.cpContent.slice(0,len);
      const params = {
        txt
      };
      audioReading(params).then(res => {
        if(res.status == 200) {
          this.audioUrl = res.request.responseURL;
        }
      })
    }
  },
  destroyed() {
    this.audioUrl = '';
  },
}
</script>
<style scoped>
.audioReading {
  position: sticky;
  left: 50%;
  bottom: 80%;

  width: max-content;
  transform: translateX(-50%);
}
</style>