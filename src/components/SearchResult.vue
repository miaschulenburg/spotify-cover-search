<template>
  <div class="result" @mouseover="hover = true" @mouseleave="hover = false" @click="downloadImage()">
    <transition name="fade">
      <div class="overlay" v-if="hover">
        <div class="icon-container">
          <div class="download icon"></div>
        </div>
      </div>
    </transition>
    <img :src="data.images[0].url" />
    <div class="info">
      <h1> {{ data.name }} </h1>
      <h2> {{ artistString }} </h2>
      <!-- <h2 v-for="a in data.artists.length" :key=data.artists[a-1].id> {{ a > 1 ? ", " : "" }} {{ data.artists[a-1].name }} </h2> -->
      <p> {{ String(data.images[0].width) + " x " + String(data.images[0].height) }} </p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SearchResult',
  props: {
    data: Object
  },
  data: function() {
    return {
      hover: false
    }
  },
  computed: {
    artistString: function () {
      if (this.data.artists.length === 0) return "";

      let string = this.data.artists[0].name;
      console.log(this.data.artists.length);
      for (var a = 1; a < this.data.artists.length; a++) {
        // console.log(a);
        string = string + ", " + this.data.artists[a].name
      }

      return string;
    }
  },
  methods: {
    downloadImage: function() {
      fetch(this.data.images[0].url)
      .then(res => res.blob())
      .then(blob => {
        const url = URL.createObjectURL(blob);

        var fileLink = document.createElement('a');   
        fileLink.href = url;
        fileLink.download = this.artistString + " - " + this.data.name;
        document.body.appendChild(fileLink);
        fileLink.click();
        document.body.removeChild(fileLink);
      })
    }
  }
}
</script>

<style scoped>
.result {
  margin-bottom: 20px;
  position: relative;
  cursor: pointer;
}

.overlay {
  position: absolute;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 4px;
  z-index: 1;
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 3px;
  backdrop-filter: blur(20px) opacity(0.7);
  -webkit-backdrop-filter: blur(20px) opacity(0.7);
  pointer-events: none;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.25s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}

.icon-container {
  position: relative;
  left: 0px;
  height: 100%;
}

.download {
  top: 50%;
  left: calc(30% - 10px);
}

.download.icon {
  color: #1ecd97;
  position: absolute;
  width: 40%;
  height: 10%;
  border-radius: 5px;
  border: solid 10px currentColor;
  border-top: none;
}

.download.icon:before {
  content: '';
  position: absolute;
  left: calc(50% - 0.5 * 10px);
  top: -120%;
  width: 10px;
  height: 140%;
  background-color: currentColor;
}

.download.icon:after {
  content: '';
  position: absolute;
  left: calc(50% - 0.5 * 30px - 0.5 * 10px);
  top: calc(20% - 30px - 4px);
  width: 30px;
  height: 30px;
  border-top: solid 10px currentColor;
  border-right: solid 10px currentColor;
  -webkit-transform: rotate(135deg);
          transform: rotate(135deg);
}

img {
  width: 100%;
  height: 100%;
  border-radius: 5px;
  box-shadow: 0px 0px 5px 1px #222;
}

.info {
  position: absolute;
  bottom: 20px;
  left: 15px;
  background-color: #000;
  border-radius: 5px;
  padding: 0px 20px;
  max-width: calc(100% - 70px);
  text-align: left;
  box-shadow: 0px 0px 5px 1px #222;
  z-index: 2;
}

h1 {
  max-width: 100%;
  margin-bottom: 10px;
  font-size: 25px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

h2 {
  max-width: 100%;
  margin-top: 0px;
  margin-bottom: 8px;
  font-size: 18px;
  display: inline-block;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

p {
  max-width: 100%;
  margin-top: 0px;
  font-style: italic;
  font-size: 15px;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>
