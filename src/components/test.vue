<template>
  <div class="test">
    xxxx
    <div>1111</div>
    <div>
      <!-- local change second -->
      local change second
    </div>
    <div>
    remote change second
    remote change second
    </div>
  </div>
</template>

<script>
export default {
  name: "test",
};
</script>

<style lang="scss" scoped>

.test{
    // 本地第一次提交白色
    color: white;
    font-size:25px;
    font-weight:300;
}
</style>
