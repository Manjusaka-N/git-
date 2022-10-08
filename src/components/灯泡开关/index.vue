<template>
  <div class="container">
    <div class="light-container">
      <div class="wire" :style="on ? 'background-color: #8f8e8e;' : 'background-color: #222;'"></div>
      <div class="light" >
        <span></span>
        <span></span>
      </div>
    </div>
    <div class="switch-container">
      <div class="switch" @click="clickSwitch">
        <div class="switch-tip">
          <div>on</div>
          <div>off</div>
        </div>
      <div :class="on?'switch-blank-on':'switch-blank-off'">
        <div class="line"></div>
      </div>
      </div>
    </div>
  </div>
</template>

<script>
import "./index.css";
export default {
  name: "BubleOpenAndClose",
  data() {
    return {
      on: true,
    };
  },
  methods: {
    clickSwitch() {
      if(this.on){
        document.documentElement.style.setProperty('--light-color','#222')
        document.documentElement.style.setProperty('--lightTop-color','#222')
      }else{
        document.documentElement.style.setProperty('--light-color','#fff')
        document.documentElement.style.setProperty('--lightTop-color','#000')
      }
      this.on = !this.on;
    },
  },
};
</script>

<style>
</style>