<template>
  <view>info page :{{ time }}</view>
</template>
<script>
export default {
  data() {
    return {
      time: Date.now()
    };
  }
};
</script>
