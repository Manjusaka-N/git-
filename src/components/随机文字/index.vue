<template>
  <div class="text-container">
    <div ref="randomText" class="random-text">
      <!-- {{ text }} -->
    </div>
    <div class="button-container">
      <div class="button" @click="beginBtn">
        <svg width="100" height="50">
          <rect class="shape" width="100" height="50"></rect>
        </svg>
        <div class="text">放</div>
      </div>
      <div class="button" @click="stopBtn">
        <svg width="100" height="50">
          <rect class="shape" width="100" height="50"></rect>
        </svg>
        <div class="text">收</div>
      </div>
    </div>
  </div>
</template>

<script>
import "./index.css";

export default {
  name: "RandomText",
  data() {
    return {
      text: "都市人的生活， 总是离不开通勤。 拥挤的地铁，颠簸的公交，匆忙的行人，毒辣的太阳， 每一件，都让人叫苦不迭。 但是你有没有发现，总有一些人， 即便是一两小时的通勤， 也能悠悠闲闲，安静自在。有人在地铁上读书， 有人在公交上看日出， 有人在马路边漫步也有人，在清晨的阳光下， 拍摄一朵花的生长。 在旁人看来，这无法理解，对他们自己来说， 这却是最简单的幸福。",
      textArr: [],
      color: [
        "#f44336",
        "#e91e63",
        "#9c27b0",
        "#673ab7",
        "#3f51b5",
        "#2196f3",
        "#03a9f4",
        "#00bcd4",
        "#009688",
        "#4caf50",
        "#8bc34a",
        "#cddc39",
        "#ffeb3b",
        "#ffc107",
        "#ff9800",
        "#ff5722",
        "#795548",
        "#9e9e9e",
        "#607d8b",
      ],
      spanArr: [],
      posRangen: {},
      resetTime: 10,
      timer: null,
    };
  },
  mounted() {
    this.splitText();
    this.spanOrigin();
    window.onresize = () => {
      this.resetpos();
    };
  },
  methods: {
    beginBtn() {
      this.run();
    },
    stopBtn() {
      this.killer();
    },
    splitText() {
      this.textArr = this.text.split("");
      this.textArr.forEach((value) => {
        let spanDom = document.createElement("span");
        spanDom.style.display = "inline-block";
        spanDom.innerHTML = value;
        spanDom.style.position = "relative";
        spanDom.style.zIndex = "1";
        spanDom.style.color = this.randomColor();
        spanDom.own = {
          pos: {
            x: 0,
            y: 0,
          },
          ran: {
            x: -0.5 + Math.random(),
            y: -0.5 + Math.random(),
          },
          speed: {
            x: 10,
            y: 10,
          },
          dir: {
            x: 1,
            y: 1,
          },
        };
        this.$refs.randomText.appendChild(spanDom);
        this.spanArr.push(spanDom);
      });
    },
    randomColor() {
      let colorIndex = Math.floor(this.color.length * Math.random());
      return this.color[colorIndex];
    },
    posRangencala() {
      return {
        minRange: {
          x: this.$refs.randomText.offsetLeft,
          y: this.$refs.randomText.offsetTop,
        },
        maxRange: {
          x:
            this.$refs.randomText.offsetLeft +
            this.$refs.randomText.offsetWidth,
          y:
            this.$refs.randomText.offsetTop +
            this.$refs.randomText.offsetHeight,
        },
      };
    },
    spanOrigin() {
      this.spanArr.forEach(function (value) {
        value.own.realPos = {
          minx: value.offsetLeft,
          maxx: value.offsetLeft + value.offsetWidth,
          miny: value.offsetTop,
          maxy: value.offsetTop + value.offsetHeight,
        };
      });
      this.posRangen = this.posRangencala();
    },
    floatText() {
      this.spanArr.forEach((span) => {
        if (
          span.own.realPos.minx + span.own.pos.x < this.posRangen.minRange.x ||
          span.own.realPos.maxx + span.own.pos.x > this.posRangen.maxRange.x
        )
          span.own.dir.x = -span.own.dir.x;
        if (
          span.own.realPos.miny + span.own.pos.y < this.posRangen.minRange.y ||
          span.own.realPos.maxy + span.own.pos.y > this.posRangen.maxRange.y
        )
          span.own.dir.y = -span.own.dir.y;
        span.own.pos.x += span.own.ran.x * span.own.speed.x * span.own.dir.x;
        span.own.pos.y += span.own.ran.y * span.own.speed.y * span.own.dir.y;
        span.style.transform =
          "translateX(" +
          span.own.pos.x +
          "px) translateY(" +
          span.own.pos.y +
          "px)";
      });
    },
    //停止时收回的速度控制
    floatBack() {
      this.spanArr.forEach((value) => {
        var x = value.own.pos.x - (value.own.pos.x * this.resetTime) / 100;
        var y = value.own.pos.y - (value.own.pos.y * this.resetTime) / 100;

        value.style.transform =
          "translateX(" + x + "px) translateY(" + y + "px)";
      });
      if (this.resetTime === 100) {
        clearInterval(this.timer);
        this.timer = null;
        this.restart();
        this.resetTime = 10;
        return true;
      } else {
        this.resetTime = this.resetTime + 5;
      }
    },
    //重新定位
    resetpos() {
      this.spanOrigin();
      // console.log(ele + "====" + this.spanArr[0].own.pos.x + "====" + this.posRangen.maxRange.x)
      this.spanArr.forEach((span) => {
        if (span.own.realPos.minx + span.own.pos.x < this.posRangen.minRange.x)
          span.own.pos.x = 0;
        if (span.own.realPos.maxx + span.own.pos.x > this.posRangen.maxRange.x)
          span.own.pos.x = 0;
        if (span.own.realPos.miny + span.own.pos.y < this.posRangen.minRange.y)
          span.own.pos.y = 0;
        if (span.own.realPos.maxy + span.own.pos.y > this.posRangen.maxRange.y)
          span.own.pos.y = 0;
      });
    },
    restart() {
      this.spanArr.forEach((value) => {
        value.own.pos.x = 0;
        value.own.pos.y = 0;
      });
    },
    run() {
      // _this.sren = requestAnimationFrame(_this.render.run);

      this.timer = setInterval(() => {
        this.floatText();
      }, 20);
    },
    // end: function () {
    //   _this.send = requestAnimationFrame(_this.render.end);
    //   _this.floatBack();
    // },
    killer() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = setInterval(() => {
          this.floatBack();
        }, 20);
      }
    },
  },
};
</script>

<style>
</style>