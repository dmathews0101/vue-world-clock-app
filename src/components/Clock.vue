<template>
  <div id="clock" style :style="style">
    <div class="clockcontainer">
      <div
        class="clockframe"
        :class="{'oneclock' : oneclock, 'twoclock' : twoclock, 'threeclock' : threeclock, 'fourclock' : fourclock, 'fiveclock' : fiveclock, 'sixclock' : sixclock }"
      >
        <div
          id="second"
          :class="{'handssix' : handssix, 'handsfive' : handsfive, 'handsfour' : handsfour, 'handsthree' : handsthree, 'handstwo' : handstwo, 'handsone' : handsone }"
          ref="secondref"
        ></div>
        <div
          id="minute"
          :class="{'handssix' : handssix, 'handsfive' : handsfive, 'handsfour' : handsfour, 'handsthree' : handsthree, 'handstwo' : handstwo, 'handsone' : handsone }"
          ref="minuteref"
        ></div>
        <div
          id="hour"
          :class="{'handssix' : handssix, 'handsfive' : handsfive, 'handsfour' : handsfour, 'handsthree' : handsthree, 'handstwo' : handstwo, 'handsone' : handsone }"
          ref="hourref"
        ></div>
      </div>
    </div>
    <h3
      style="color: black;"
      :class="{'clockssixfs' : clockssixfs, 'clocksfivefs' : clocksfivefs, 'clocksfourfs' : clocksfourfs, 'clocksthreefs' : clocksthreefs, 'clockstwofs' : clockstwofs, 'clocksonefs' : clocksonefs }"
    >{{clock.city}}</h3>
  </div>
</template>
<script>
export default {
  props: {
    clock: {
      type: Object,
      required: false,
    },
    clocksnum: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      hourhandtime: '',
      minutehandtime: '',
      secondhandtime: '',
      style: {},
      oneclock: false,
      twoclock: false,
      threeclock: false,
      fourclock: false,
      fiveclock: false,
      sixclock: false,
      utcoffset: '',
      handsone: false,
      handstwo: false,
      handsthree: false,
      handsfour: false,
      handsfive: false,
      handssix: false,
      clocksonefs: false,
      clockstwofs: false,
      clocksthreefs: false,
      clocksfourfs: false,
      clocksfivefs: false,
      clockssixfs: false,
      timer: null,
      numberofclocks: this.$props.clocksnum,
    }
  },
  methods: {
    setClockTime: function () {
      var utcoffsetTime = parseInt(this.utcoffset)

      const day = new Date()
      var utc_offset = day.getTimezoneOffset()
      day.setMinutes(day.getMinutes() + utc_offset)
      day.setMinutes(day.getMinutes() - utcoffsetTime)
      const hh = day.getHours() // 0 - 23
      const mm = day.getMinutes() // 0- 59
      const ss = day.getSeconds() // 0 - 59
      const hourDeg = hh * 30 + mm * 0.5 //every 60min, 30Deg then 30/60 = 0.5 per minute
      const minuteDeg = mm * 6 + ss * 0.1 // Every 60 second, 6deg then 6/60 = 0.1 per sec
      const secondDeg = ss * 6
      this.$refs.hourref.style.transform = `rotateZ(${hourDeg}deg)`
      this.$refs.minuteref.style.transform = `rotateZ(${minuteDeg}deg)`
      this.$refs.secondref.style.transform = `rotateZ(${secondDeg}deg)`
    },
    setStyle() {
      let style = {}
      if (this.$props.clocksnum == 1) {
        this.oneclock = true
        this.handsone = true
        this.clocksonefs = true
      } else if (this.$props.clocksnum == 2) {
        this.twoclock = true
        if (window.innerHeight > window.innerWidth) {
          style.width = '60vw'
        }
        this.handstwo = true
        this.clockstwofs = true
      } else if (this.$props.clocksnum == 3) {
        this.threeclock = true
        if (window.innerHeight > window.innerWidth) {
          style.width = '60vw'
        }
        this.handsthree = true
        this.clocksthreefs = true
      } else if (this.$props.clocksnum == 4) {
        style.width = '50%'
        this.fourclock = true
        if (window.innerHeight > window.innerWidth) {
          style.width = '40vw'
        }
        this.handsfour = true
        this.clocksfourfs = true
      } else if (this.$props.clocksnum == 5) {
        style.width = '45vh'
        this.fiveclock = true
        if (window.innerHeight > window.innerWidth) {
          style.width = '40vw'
        }
        this.handsfive = true
        this.clocksfivefs = true
      } else if (this.$props.clocksnum == 6) {
        style.width = '30%'
        this.sixclock = true
        if (window.innerHeight > window.innerWidth) {
          style.width = '40vw'
        }
        this.handssix = true
        this.clockssixfs = true
      } else {
        style.width = '50%'
      }
      return style
    },
  },
  mounted: function () {
    this.utcoffset = this.$props.clock.utcoffset
    this.setClockTime()
    this.style = this.setStyle()
    this.timer = setInterval(() => this.setClockTime(), 1000)
  },
  beforeDestroy() {
    clearInterval(this.timer)
  },
}
</script>
<style scoped>
.clockframe {
  width: 16.625rem;
  height: 16.625rem;
  background-color: white;
  margin: 0 auto;
  /* margin-top: 2.5rem; */
  border-radius: 50%;
  border: 0.625rem solid black;
  box-shadow: inset 0rem 0rem 0.125rem 0.0625rem #0000001f,
    0rem 0rem 0.125rem 0.0625rem #00000026;
  background-image: url('../assets/clock_face.png');
  background-repeat: no-repeat;
  background-size: contain;
  position: relative;
}
.clockframe::after {
  content: '';
  display: block;
  width: 0.5rem;
  height: 0.5rem;
  background: black;
  border-radius: 50%;
  left: 50%;
  top: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
  border: 0.125rem solid white;
  z-index: 4;
}
#second,
#minute,
#hour {
  position: absolute;
  width: 100%;
  height: 100%;
}
#second {
  z-index: 3;
}
#minute {
  z-index: 2;
}
#hour {
  z-index: 1;
}

#second::before,
#minute::before,
#hour::before,
#second::after {
  content: '';
  display: block;
  left: 50%;
  top: 50%;
  position: absolute;
  transform: translate(-50%, -100%);
  border-radius: 0.625rem;
}
#second::before {
  width: 0.0625rem;
  height: 7.5rem;
  background: red;
}
#second::after {
  width: 0.0625rem;
  height: 1.875rem;
  background: red;
  transform: translate(-50%, 0);
}
#minute::before {
  width: 0.125rem;
  height: 6.25rem;
  background: gray;
}
#hour::before {
  width: 0.25rem;
  height: 4.375rem;
  background: #575656;
}
#digitalClock {
  position: absolute;
  z-index: 1;
  font-family: cursive;
  background: #f3f2f2;
  padding: 0 0.375rem 0 0.375rem;
  font-size: 0.75rem;
  top: 50%;
  left: 60%;
  transform: translateY(-50%);
  box-shadow: inset 0rem 0rem 0.125rem 0.125rem #0000001a;
  border-radius: 0.125rem;
  text-align: center;
}
.oneclock {
  width: 37.5rem;
  height: 37.5rem;
  border: 1.5rem solid black;
  box-shadow: inset 0rem 0rem 0.125rem 0.0625rem #0000001f,
    0rem 0rem 0.125rem 0.0625rem #00000026;
}
.twoclock {
  width: 25rem;
  height: 25rem;
  border: 1.2rem solid black;
  box-shadow: inset 0rem 0rem 0.125rem 0.0625rem #0000001f,
    0rem 0rem 0.125rem 0.0625rem #00000026;
}
.threeclock {
  width: 22.5rem;
  height: 22.5rem;
  border: 1rem solid black;
  box-shadow: inset 0rem 0rem 0.125rem 0.0625rem #0000001f,
    0rem 0rem 0.125rem 0.0625rem #00000026;
}
.fourclock {
  width: 16.625rem;
  height: 16.625rem;
  border: 0.8rem solid black;
  box-shadow: inset 0rem 0rem 0.125rem 0.0625rem #0000001f,
    0rem 0rem 0.125rem 0.0625rem #00000026;
}
.fiveclock {
  width: 17.1875rem;
  height: 17.1875rem;
  border: 0.8rem solid black;
  box-shadow: inset 0rem 0rem 0.125rem 0.0625rem #0000001f,
    0rem 0rem 0.125rem 0.0625rem #00000026;
}
.sixclock {
  width: 16.625rem;
  height: 16.625rem;
  border: 0.8rem solid black;
  box-shadow: inset 0rem 0rem 0.125rem 0.0625rem #0000001f,
    0rem 0rem 0.125rem 0.0625rem #00000026;
}

/* Clock Hand 1-6 CSS */
.handssix#second::before {
  width: 0.2rem;
  height: 6.1rem;
  background: red;
}
.handssix#second::after {
  width: 0.2rem;
  height: 1.2rem;
  background: red;
  transform: translate(-50%, 0);
}
.handssix#minute::before {
  width: 0.17rem;
  height: 5.8rem;
  background: gray;
}
.handssix#hour::before {
  width: 0.48rem;
  height: 3.2rem;
  background: #575656;
}

.handsfive#second::before {
  width: 0.2rem;
  height: 6.2rem;
  background: red;
}
.handsfive#second::after {
  width: 0.2rem;
  height: 1.2rem;
  background: red;
  transform: translate(-50%, 0);
}
.handsfive#minute::before {
  width: 0.25rem;
  height: 5.9rem;
  background: gray;
}
.handsfive#hour::before {
  width: 0.54rem;
  height: 3.3rem;
  background: #575656;
}
.handsfour#second::before {
  width: 0.2rem;
  height: 5.9rem;
  background: red;
}
.handsfour#second::after {
  width: 0.2rem;
  height: 1.2rem;
  background: red;
  transform: translate(-50%, 0);
}
.handsfour#minute::before {
  width: 0.2rem;
  height: 5.5rem;
  background: gray;
}
.handsfour#hour::before {
  width: 0.48rem;
  height: 3.4rem;
  background: #575656;
}
.handsthree#second::before {
  width: 0.3rem;
  height: 8rem;
  background: red;
}
.handsthree#second::after {
  width: 0.3rem;
  height: 1.5rem;
  background: red;
  transform: translate(-50%, 0);
}
.handsthree#minute::before {
  width: 0.3rem;
  height: 7.75rem;
  background: gray;
}
.handsthree#hour::before {
  width: 0.7rem;
  height: 4.3rem;
  background: #575656;
}
.handstwo#second::before {
  width: 0.35rem;
  height: 8.9rem;
  background: red;
}

.handstwo#second::after {
  width: 0.35rem;
  height: 1.7rem;
  background: red;
}

.handstwo#minute::before {
  width: 0.35rem;
  height: 8.5rem;
  background: gray;
}
.handstwo#hour::before {
  width: 0.7rem;
  height: 5.2rem;
  background: #575656;
}
.handsone#second::before {
  width: 0.4rem;
  height: 13.7rem;
  background: red;
}
.handsone#second::after {
  width: 0.4rem;
  height: 2.8rem;
  background: red;
  transform: translate(-50%, 0);
}
.handsone#minute::before {
  width: 0.5rem;
  height: 12.5rem;
  background: gray;
}
.handsone#hour::before {
  width: 1rem;
  height: 7.6rem;
  background: #575656;
}

/* Clock City Name Font Size */

.clockssixfs {
  font-size: 2.4rem;
}
.clocksfivefs {
  font-size: 2.4rem;
}
.clocksfourfs {
  font-size: 2.8rem;
}
.clocksthreefs {
  font-size: 4.8rem;
}
.clockstwofs {
  font-size: 5rem;
}
.clocksonefs {
  font-size: 6.5rem;
}
</style>
