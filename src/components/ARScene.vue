<template>
  <a-scene ref="a-scene" :mindar-image="mindArImage()"
           color-space="sRGB"
           device-orientation-permission-ui="enabled: false"
           loading-screen="enabled:false;"
           renderer="colorManagement: true, physicallyCorrectLights"
           vr-mode-ui="enabled: false">
    <a-assets>
      <a-asset-item v-for="asset in assets" :id="asset.id" :src="asset.src"></a-asset-item>
    </a-assets>
    <a-camera look-controls="enabled: false" position="0 0 0"></a-camera>
    <a-entity v-for="(n,i) in getNumberOfTargets()" :mindar-image-target="'targetIndex:'+i">
      <AModel v-for="model in modelsInTargets[i]" :key="model.id" :ref="model.id" :modelData="model"/>
    </a-entity>
  </a-scene>
</template>
<script>
import AModel from "./AModel.vue";
import Logo from "./Logo.vue";

export default {
  name: "ARScene",
  components: {
    Logo,
    AModel,
  },
  props: {
    assets: {
      type: Array,
      required: true,
    },
    modelsInTargets: {
      type: Array,
      required: true,
    },
    mindarImage : {
      type: Object,
      required: true,
    },
  },
  methods: {
    // mindar-image property
    mindArImage() {
      return "imageTargetSrc: " + this.mindarImage.targetSrc
          + "; maxTrack: " + this.getNumberOfTargets()
          + "; filterMinCF:" + this.mindarImage.filterMinCF
          + "; filterBeta: " + this.mindarImage.filterBeta + ";";
    },
    getNumberOfTargets() {
      return this.modelsInTargets.length;
    },
  },
}
</script>