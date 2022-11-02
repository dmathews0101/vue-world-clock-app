
<template>
  <div id="app">
    <div :class="selectedBackground">
      <div class="top-container">
        <div :class="{'transparentbox' : transparentboxx}">
          <div class="flex-container" v-if="reRenderMyComponent">
            <app-clock
              class="flex-item"
              v-for="(clock, i) in clocks"
              :clock="clock"
              :key="i"
              :clocksnum="noc"
            ></app-clock>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Clock from './components/Clock.vue'
import { CityOffsets } from './assets/CityOffsets'
import { ConfigLoader } from '@igappstore/core'

export default {
  name: 'App',
  data() {
    return {
      config: window.MicroAppConfiguration,
      tempconfig: window.tempconfig,
      clocks: window.tempconfig.clocks,
      cityoffsets: CityOffsets,
      noc: window.tempconfig.clocks.length,
      selectedBackground: 'rockwall',
      transparentboxx: true,
      reRenderMyComponent: true,
    }
  },
  watch: {
    config(newVal, oldVal) {
      let res = []
      for (let key in this.config) {
        if (this.config[key].toLowerCase() != 'none') {
          this.cityoffsets.forEach((item, i) => {
            if (this.config[key].toLowerCase() == item.city.toLowerCase()) {
              if (!res.find((itm) => itm.city === item.city))
                res.push({ city: item.city, utcoffset: item.utcoffset })
            }
          })
        }
      }
      this.tempconfig.clocks = ''
      this.tempconfig.clocks = res
      this.clocks = ''
      this.clocks = this.tempconfig.clocks
      this.noc = this.clocks.length
      this.reRender()
    },
  },
  methods: {
    async checkUrl() {
      const config = await ConfigLoader.loadConfig()
      this.config = config
    },
    reRender() {
      this.reRenderMyComponent = false
      this.$nextTick(() => {
        this.reRenderMyComponent = true
      })
    },
  },
  components: {
    'app-clock': Clock,
  },
  async created() {
    await this.checkUrl()
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  width: 100vw;
  height: 100vh;
  font-family: YatraOne;
}

html {
  font-size: 1vw;
}

@font-face {
  src: url(assets/fonts/YatraOne-Regular.ttf);
  font-family: YatraOne;
}

body {
  width: 100%;
  height: 100%;
  overflow: hidden;
  font-family: YatraOne;
}

.flex-container {
  display: flex;
  justify-content: space-around;
  height: 90vh;
  align-items: center;
  flex-wrap: wrap;
}

.flex-item {
  text-align: center;
}

.rockwall {
  background-image: url('assets/rockwall.jpg');
  background-color: white;
  height: 100vh;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

.top-container {
  width: 100vw;
  height: 100vh;
  display: table-cell;
  vertical-align: middle;
}

.transparentbox {
  width: 90vw;
  height: 90vh;
  background: rgba(255, 255, 255, 0.4);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
  color: black;
  margin: auto;
}
</style>