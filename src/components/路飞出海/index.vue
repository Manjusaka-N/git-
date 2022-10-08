<template>
  <div>
    <div class="sea">
      <div class="surface"></div>

      <div class="ship">
        <div class="rotate">
          <div class="move">
            <div class="body">
              <div class="waves">
                <!-- 船体破浪 -->
                <div class="bodywaves">
                  <div
                    class="wave"
                    v-for="i in 50"
                    :key="i"
                    :style="{
                      '--left': randomNum(12, 60) + 'px',
                      '--top': randomNum(-2, 16) + 'px',
                      '--delay': randomNum(30, 4000) + 'ms',
                    }"
                  >
                    <div
                      class="graphic"
                      :style="{
                        '--width': randomNum(6, 10) + 'px',
                        '--height': randomNum(6, 10) + 'px',
                      }"
                    ></div>
                  </div>
                </div>
                <!-- 左船桨破浪 -->
                <div class="oarwaves left">
                  <div
                    class="wave"
                    v-for="i in 20"
                    :key="i"
                    :style="{
                      '--left': randomNum(40, 50) + 'px',
                      '--top': randomNum(-30, -15) + 'px',
                      '--delay': randomNum(1000, 1800) + 'ms',
                    }"
                  >
                    <div
                      class="graphic"
                      :style="{
                        '--width': randomNum(3, 6) + 'px',
                        '--height': randomNum(3, 6) + 'px',
                      }"
                    ></div>
                  </div>
                </div>
                <!-- 右船桨破浪 -->
                <div class="oarwaves right">
                  <div
                    class="wave"
                    v-for="i in 20"
                    :key="i"
                    :style="{
                      '--left': randomNum(40, 50) + 'px',
                      '--top': randomNum(40, 55) + 'px',
                      '--delay': randomNum(1000, 1800) + 'ms',
                    }"
                  >
                    <div
                      class="graphic"
                      :style="{
                        '--width': randomNum(3, 6) + 'px',
                        '--height': randomNum(3, 6) + 'px',
                      }"
                    ></div>
                  </div>
                </div>
              </div>
              <!-- 船体 -->
              <div class="base"></div>
              <!-- 前木板 -->
              <div class="board front"></div>
              <!-- 后木板 -->
              <div class="board back"></div>
            </div>
            <!-- 船桨 -->
            <div class="oars">
              <!-- 左船桨 -->
              <div class="oar left">
                <div class="row left">
                  <div class="depth left">
                    <div class="graphic left"></div>
                  </div>
                </div>
              </div>
              <!-- 右船桨 -->
              <div class="oar right">
                <div class="row right">
                  <div class="depth right">
                    <div class="graphic right"></div>
                  </div>
                </div>
              </div>
            </div>
            <!-- 路飞 -->
            <div class="human">
              <div class="legs">
                <!-- 左腿 -->
                <div class="leg left"></div>
                <!-- 右腿 -->
                <div class="leg right"></div>
              </div>
              <!-- 帽子 -->
              <div class="hat"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "./路飞出海.css";
export default {
  name: "magicSail",
  methods: {
    randomNum(min, max) {
      const num = (Math.random() * (max - min + 1) + min).toFixed(2);
      return num;
    },
  },
};
</script>

<style>
</style>