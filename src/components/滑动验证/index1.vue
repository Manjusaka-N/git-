<template>
  <div id="app" v-cloak>
    <div class="verify-container" :style="{ width: `${width}px` }">
      <div class="refresh" @click="reset">
        <svg
          t="1637315258145"
          class="icon"
          viewBox="0 0 1024 1024"
          version="1.1"
          xmlns="http://www.w3.org/2000/svg"
          p-id="2420"
          width="20"
          height="20"
        >
          <path
            d="M960 416V192l-73.056 73.056a447.712 447.712 0 0 0-373.6-201.088C265.92 63.968 65.312 264.544 65.312 512S265.92 960.032 513.344 960.032a448.064 448.064 0 0 0 415.232-279.488 38.368 38.368 0 1 0-71.136-28.896 371.36 371.36 0 0 1-344.096 231.584C308.32 883.232 142.112 717.024 142.112 512S308.32 140.768 513.344 140.768c132.448 0 251.936 70.08 318.016 179.84L736 416h224z"
            p-id="2421"
            fill="#8a8a8a"
          ></path>
        </svg>
      </div>
      <div class="pic">
        <canvas
          class="canvas_img"
          ref="canvas_img"
          :width="width"
          :height="height"
        ></canvas>
        <canvas
          class="canvas_block"
          ref="canvas_block"
          :width="width"
          :height="height"
          :style="{ left: blockLeft + 'px' }"
        ></canvas>
      </div>
      <div class="slider" :style="{ height: blockW + 'px' }">
        <div class="tip" v-if="showText">向右滑动完成验证</div>
        <div
          :class="['bar', slideState]"
          :style="{ width: sliderLeft + 'px' }"
        ></div>
        <div
          :class="['slider-icon', slideState]"
          :style="{ left: sliderLeft + 'px' }"
          @mousedown="mouseDown"
          @mousemove="mouseMove"
          @mouseup="mouseUp"
        >
          {{ { active: ">", fail: "x", success: "√" }[slideState] || "->" }}
        </div>
        <!-- <div ref="slider-icon"
					:class="['slider-icon', slideState]"
					:style="{left: sliderLeft + 'px'}">
					>
				</div> -->
      </div>
    </div>
    <br />
  </div>
</template>

<script>
import "./index.css";
export default {
  name: "SliderCheck",
  data() {
    return {
      width: 320,
      height: 160,
      blockW: 40, // 裁剪canvas宽高
      accuracy: 5, // 精度,
      images: [
        "https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg.jj20.com%2Fup%2Fallimg%2Ftp09%2F21031FKU44S6-0-lp.jpg&refer=http%3A%2F%2Fimg.jj20.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1665820020&t=810f2e612695fe626b44b321a9d7f9b7",
        // "https://img2.baidu.com/it/u=172118635,3843440198&fm=26&fmt=auto",
        // "https://img2.baidu.com/it/u=2726247805,538885610&fm=26&fmt=auto",
        // "https://img1.baidu.com/it/u=1078976348,1462740125&fm=26&fmt=auto",
      ],
      bgImg: null, // 背景图
      ctxImg: null, // 背景画笔
      ctxBlock: null, // 滑块画笔
      blockRect: {
        // 滑块宽、圆半径、坐标
        w: 60,
        r: 5,
        x: 0,
        y: 0,
      },
      blockLeft: 0, // 裁剪后left属性
      startX: 0, // 滑动起点
      EndX: 0, // 结束位置
      sliderLeft: 0, // 拖动滑块的滑动距离
      slideState: "", // success fail active
      timeIns: null,
      showText: true, // 是否显示滑动提示
      isMouseDown: false,
    };
  },
  mounted() {
    console.log(this.blockRect);
    this.init();

    // 如果是pc端则用mouse事件
    // this.mouseEvent();
  },
  beforeDestroy() {
    clearTimeout(this.timeIns);
  },
  methods: {
    //初始化生成两块画布
    init() {
      this.ctxImg = this.$refs["canvas_img"].getContext("2d");
      this.ctxBlock = this.$refs["canvas_block"].getContext("2d");

      this.getImg();
    },
    getImg() {
      const img = document.createElement("img");
      //   const imagesLen = this.images.length;
      //   const randomIndex = Math.floor(Math.random() * imagesLen);
      img.crossOrigin = "Anonymous"; //这个枚举属性表明是否必须使用 CORS 完成相关图像的抓取  Anonymous执行一个跨域请求
      //   img.src = this.images[randomIndex];
      //   img.src = this.images[0];
      img.src = require("./modal-title.png");
      this.bgImg = img;

      img.onload = () => {
        console.log("图片加载完成");
        //将图片放到画布上面
        this.ctxImg.drawImage(this.bgImg, 0, 0, this.width, this.height);
        this.getBlockPostion();
        this.ctxBlock.drawImage(this.bgImg, 0, 0, this.width, this.height);

        // console.log(this.blockRect.x, this.blockRect.y, this.blockRect.w)
        const _yPos = this.blockRect.y - 2 * this.blockRect.r;
        console.log(
          this.blockRect,
          _yPos,
          this.blockRect.w,
          this.blockRect.w + 1
        );
        const imageData = this.ctxBlock.getImageData(
          this.blockRect.x,
          _yPos,
          this.blockRect.w,
          this.blockRect.w + 1
        );
        this.$refs["canvas_block"].width = this.blockRect.w;
        this.ctxBlock.putImageData(imageData, 0, _yPos);
      };
      console.log(this.bgImg);
    },
    getBlockPostion() {
      //x轴偏移量在宽度的一半到最大值之间
      const xPos = Math.floor(
        this.width*0.3 + Math.random() * (this.width *0.7 - this.blockRect.w)
      );
      //y轴偏移量在40以上，避免超出画布
      const yPos = Math.floor(
        10 + Math.random() * (this.height - this.blockRect.w - 10)
      );
      // console.log(xPos, yPos)
      this.blockRect.x = xPos;
      this.blockRect.y = yPos;

      this.draw(this.ctxImg, "fill");
      this.draw(this.ctxBlock, "clip");
    },
    //绘制
    draw(ctx, operation) {
      const { r, x, y } = this.blockRect;
      const _w = this.blockW;
      ctx.beginPath(); //通过清空子路径列表开始一个新路径的方法
      ctx.moveTo(x, y); //移动
      //绘制圆弧路径的方法。圆弧路径的圆心在 (x, y) 位置，半径为 r
      ctx.arc(x + _w / 2, y - r + 2, r, 0.72 * Math.PI, 2.26 * Math.PI);
      //使用直线连接子路径的终点到 x，y 坐标的方法（并不会真正地绘制）
      ctx.lineTo(x + _w, y);
      ctx.arc(x + _w + r - 2, y + _w / 2, r, 1.21 * Math.PI, 2.78 * Math.PI);
      ctx.lineTo(x + _w, y + _w);
      ctx.lineTo(x, y + _w);
      ctx.arc(
        x + r - 2,
        y + _w / 2,
        r + 0.4,
        2.76 * Math.PI,
        1.24 * Math.PI,
        true
      );
      ctx.lineTo(x, y);
      //笔点返回到当前子路径起始点的方法
      ctx.closePath();
      //使用内部方式描述颜色和样式的属性,默认black
      ctx.fillStyle = "rgba(225, 225, 225, 0.8)";
      //API 描述画笔（绘制图形）颜色或者样式的属性
      ctx.strokeStyle = "rgba(225, 225, 225, 0.8)";
      ctx.lineWidth = 2;
      //根据当前的画线样式，绘制当前或已经存在的路径的方法
      ctx.stroke();
      // fill根据当前的填充样式，填充当前或已存在的路径的方法,clip将当前创建的路径设置为当前剪切路径的方法
      ctx[operation]();
    },
    // pc
    mouseEvent() {
        this.$refs["slider-icon"].addEventListener("mousedown", this.mouseDown);
        this.$refs["slider-icon"].addEventListener("mouseup", this.mouseUp);
      // document.addEventListener("mousemove", this.mouseMove);
      // document.addEventListener("mouseup", this.mouseUp);
    },
    mouseDown(e) {
      console.log(e);
      this.startX = e.pageX;
      this.showText = false;
      this.isMouseDown = true;
    },
    mouseMove(e) {
      if (!this.isMouseDown) {
        return;
      }
      // console.log(e)
      this.endX = e.pageX - this.startX;
      // 禁止超出边界
      if (this.endX < 0 || this.endX > this.width - this.blockW) {
        return;
      }
      // 拖动的距离
      this.sliderLeft = this.endX;
      this.blockLeft =
        (this.sliderLeft / (this.width - this.blockW)) *
        (this.width - this.blockRect.w);
      this.slideState = "active";
    },
    mouseUp(e) {
      if (!this.isMouseDown || this.startX === e.clientX) {
        return;
      }
      this.isMouseDown = false;
      const isPass = this.verify();
      console.log(isPass);
      if (isPass) {
        this.slideState = "success";
         // 如果失败则1000毫秒后重置
         this.timeIns = setTimeout(() => {
          this.reset();
        }, 1000);
      } else {
        this.slideState = "fail";
        // 如果失败则1000毫秒后重置
        this.timeIns = setTimeout(() => {
          this.reset();
        }, 1000);
      }
    },
    // mobile
    touchStart(e) {
      // console.log(e)
      this.startX = e.changedTouches[0].pageX;
      this.showText = false;
    },
    touchMove(e) {
      this.endX = e.changedTouches[0].pageX - this.startX;
      // 禁止超出边界
      if (this.endX < 0 || this.endX > this.width - this.blockW) {
        return;
      }
      // 拖动的距离
      this.sliderLeft = this.endX;
      this.blockLeft =
        (this.sliderLeft / (this.width - this.blockW)) *
        (this.width - this.blockRect.w);
      this.slideState = "active";
    },
    touchEnd() {
      const isPass = this.verify();
      console.log(isPass);
      if (isPass) {
        this.slideState = "success";
      } else {
        this.slideState = "fail";
        // 如果失败则1000毫秒后重置
        this.timeIns = setTimeout(() => {
          this.reset();
        }, 1000);
      }
    },
    // 判断精度
    verify() {
      console.log(Math.abs(this.blockLeft - this.blockRect.x));
      return Math.abs(this.blockLeft - this.blockRect.x) <= this.accuracy;
    },
    // 重置
    reset() {
      this.showText = true;
      this.slideState = "";
      this.sliderLeft = 0;
      this.blockLeft = 0;
      this.$refs["canvas_block"].width = this.width;
      this.ctxImg.clearRect(0, 0, this.width, this.height);
      this.ctxBlock.clearRect(0, 0, this.width, this.height);
      this.getImg();
    },
  },
};
</script>

<style>
</style>