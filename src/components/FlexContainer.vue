<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "FlexContainer",
};
</script>

<style scoped>
div {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  width: 100%;
}
</style>