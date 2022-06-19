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
<<<<<<< HEAD
    font-weight: 500;
=======
    font-weight:600;
>>>>>>> a72f2eb9d16e73cc97c7ea201188cde072be7268
}
</style>
