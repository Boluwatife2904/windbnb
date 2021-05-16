<template>
  <header class="header">
    <h3>Stays in Finland</h3>
    <p>{{ length }}+ stays</p>
  </header>
</template>

<script>
export default {
  name: "ApartmentsHeader",
  props: ["length"],
};
</script>

<style lang="scss" scoped>
.header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 30px;

  h6 {
    font-weight: bold;
    font-size: 24px;
    line-height: 29px;
    color: #333333;
  }

  p {
    font-weight: 600;
    font-size: 14px;
    line-height: 17px;
    color: #4f4f4f;
  }
}
</style>