<template>
  <div id="audiopath">
    <div :id="'mapblob-' + thisData.id" class="mapblob" v-for="p in pathPoints"  :key="p.x + p.y" :style="{ transform: 'translate(' + p.x + 'px, ' + p.y + 'px)'}">
      <AudioBlob :id="thisData.id" viewmode="mapview" />
    </div>
  </div>
</template>

<script>

/*  eslint-disable */ 

import AudioBlob from "@/components/AudioBlob.vue";

import { SVG } from '@svgdotjs/svg.js'

export default {
  name: "AudioPath",
  props: ['id'],
  data() {
    return {
    };
  },
  components: {
    AudioBlob
  },
  computed: {
    audiopathData() {
      return this.$store.getters.audiopathData;
    },
    thisData() {
      return this.audiopathData[this.id];
    },
    svgElem() {
      return SVG(this.thisData.elem);
    },
    pathPoints() {
      var points = [];
      if(this.id.includes("-B")) {
        points.push( this.svgElem.pointAt(this.svgElem.length()) );
      } else {
        points.push( this.svgElem.pointAt(0) );
      }
      return points;
    },
  },
  methods: {
    onclick(e) {
      console.log("ya clicked me", e);
    }
  },
  mounted() {
  },
};
</script>
<style scoped lang="scss">
.mapblob{
  position: absolute;
  z-index: 10000;
}


</style>


