<template>
  <main class="main">
    <button
        class="startBtn"
        @click="appStart"
        v-if="isStarted==false">
        Start!
    </button>
    <section
        @mousedown="ringStart"
        @touchstart="ringStart"
        @mouseup="ringStop"
        @touchend="ringStop"
        @mouseleave="ringStop"
        @touchleave="ringStop"
        @mousemove="ring"
        @touchmove="ring"
        class="touchZone"
        v-else
    >
    </section>
  </main>
</template>

<script>
import Tone from 'tone'

export default {
  data() {
    return {
      isStarted: false,
      synth: null, // シンセサイザー
      isRings: false // 今シンセサイザーが鳴っているかを表す
    };
  },
  methods: {
    // スタートボタンが押されたら発火
    appStart() {
      this.isStarted = true;
      this.synth = new Tone.MonoSynth().toMaster();
    },
    // マウスクリック時
    ringStart() {
      this.isRings = true;
      this.ring()
    },
    // マウスクリック終了 または マウスが画面を離れた時
    ringStop() {
      this.isRings = false;
      this.ring()
    },
    // isRings時の処理
    ring(event) {

      var x = 0;
      var y = 0;

      if (typeof event !== "undefined") {
          if (typeof event.touches !== "undefined" && event.touches.length > 0) {
              // スマートフォンと判断する
              x = - event.touches[0].clientX;
              y = event.touches[0].clientY;
          } else {
              // マウスだと判断する
              x = event.clientX;
              y = event.clientY;
          }
      }

      console.log(x, y);

      if (this.isRings) {
        this.synth.triggerAttack("C3");
      } else {
        this.synth.triggerRelease();
      }
    }
  }
}
</script>

<style>
.main {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.startBtn {
  padding: 10px 20px;
}

.touchZone {
  background: linear-gradient( 45deg, rgb(255,255,255),rgb(100,255,255) );
  width: 100%;
  min-width: 100%;
  height: 100vh;
  min-height: 100vh;
}
</style>
