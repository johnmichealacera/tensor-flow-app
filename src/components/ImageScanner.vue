<template>
  <div>
    <div>
        <label for="avatar">Choose a picture:</label>
        <input @change="getChange($event)" type="file" id="avatar" name="avatar" accept="image/*">
    </div>
    <div class="img-container">
      <img id="img_to_detect">
      <canvas id="detect_result" v-if="!isLoadingScannedImage"></canvas>
    </div>
    <ImageLoader :isloadingScannedImage="isLoadingScannedImage" />
  </div>
</template>

<script>
// import modules for object detection
import '@tensorflow/tfjs-backend-cpu';
import '@tensorflow/tfjs-backend-webgl';
import * as cocoSsd from '@tensorflow-models/coco-ssd';
import { ref } from 'vue';
import ImageLoader from './ImageLoader.vue';
export default {
  name: 'ImageScanner',
  props: {
    msg: String
  },
  components: {
    ImageLoader,
  },
  setup() {
    let cocoSsdModel = null;
    const isLoadingScannedImage = ref(false);

    const getChange = (e) => {
      isLoadingScannedImage.value = true;
      const url = URL.createObjectURL(e.target.files[0])
      const img = document.getElementById('img_to_detect')
      img.src = url
      getDetect(img);
      isLoadingScannedImage.value = false;
    };

    const getDetect = async (img) => {
      // get detection result
      cocoSsdModel = await cocoSsd.load();
      const result = await cocoSsdModel.detect(img);
      const canvas = document.getElementById('detect_result');
      console.log('img.width', img.width);
      const color=["red","white","blue"]
      canvas.width=img.width ;
      canvas.height=img.height ;
      const context = canvas.getContext('2d');
      context.drawImage(img, 0, 0, img.width,img.height);
      context.font = '40px Arial';

      for (let i = 0; i < result.length; i++) {
          context.beginPath();
          context.rect(...result[i].bbox);
          context.lineWidth = 5;
          context.strokeStyle = color[i%3];
          context.fillStyle = color[i%3];
          context.font = "10px Verdana";
          context.stroke();
          context.fillText(
            result[i].score.toFixed(3) + ' ' + result[i].class, 
            result[i].bbox[0] + 5,
            result[i].bbox[1] + 15);
      }
      return result
    };

    return {
      getChange,
      getDetect,
      isLoadingScannedImage,
    };
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#img_to_detect {
  object-fit: cover;
}
/* Styles for desktop screens */
@media screen and (min-width: 768px) {
  .img-container {
    width: 100vw;
    display: flex;
    align-items: center;
  }
  #img_to_detect {
    width: 50%;
    object-fit: cover;
  }
  #detect_result {
    width: 50%;
  }
}
/* Styles for mobile screens */
@media screen and (max-width: 767px) {
  #img_to_detect {
    object-fit: cover;
    width: 100%;
    height: 100%;
  }
  #img_to_detect {
    height: 50%;
  }
  #detect_result {
    height: 50%;
  }
}
</style>
