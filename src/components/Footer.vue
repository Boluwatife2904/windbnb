<template>
  <!-- FOOTER -->
  <footer>
    <p>
      Created by
      <span class="username"
        ><a href="https://twitter.com/developerBolu">Boluwatife</a></span
      >
      -
      <span class="devchallenges"
        ><a href="https://devchallenges.io">devChallenges.io</a></span
      >
    </p>
  </footer>
</template>

<script>
export default {
  name: "Footer"
};
</script>

<style lang="scss">
// FOOTER
footer {
  text-align: center;
  margin-top: 30px;
  margin-bottom: 20px;

  p {
    margin: 0;
    font-weight: 500;
    font-size: 14px;
    line-height: 17px;
    color: #828282;

    .username {
      font-weight: 700;
    }

    a {
      color: inherit;

      &:focus {
        outline: none;
      }
    }
  }
}
</style>