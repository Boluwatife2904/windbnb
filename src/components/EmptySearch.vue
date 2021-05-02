<template>
  <!-- EMPTY SCREEN WHEN NO APARTMENT IS FOUND -->
  <div class="no-apartment-found">
    <img src="@/assets/error.png" alt="Error Image" />
    <h6>No Result Found</h6>
    <p>
      We've searched more than 350 hotels and did not find any hotels for your
      search
    </p>
    <button @click="$emit('clear-search')">Search Again</button>
  </div>
</template>

<script>
export default {
  name: "EmptySearch",
  emits: ["clear-search"]
};
</script>

<style lang="scss">
//  EMPTY SCREEN
.no-apartment-found {
  max-width: 400px;
  width: 90%;
  margin: 10px auto 20px;
  text-align: center;

  img {
    height: 200px;
    display: block;
    margin: 0 auto 20px;
  }

  h6 {
    font-weight: 600;
    font-size: 20px;
    margin-bottom: 14px;
  }

  p {
    color: #222;
    margin-bottom: 14px;
    line-height: 30px;
  }

  button {
    border: none;
    background: #eb5757;
    color: #fff;
    font: inherit;
    border-radius: 6px;
    padding: 14px 30px;
    cursor: pointer;
    font-weight: 500;

    :focus {
      outline: none;
    }
  }
}
</style>