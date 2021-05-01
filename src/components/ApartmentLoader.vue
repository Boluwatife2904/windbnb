<template>
  <div class="loader">
    <div class="loader-image"></div>
    <div class="other-loaders">
      <div class="first-set">
        <div class="loader-box loader-one"></div>
        <div class="loader-box loader-two"></div>
      </div>
      <div class="loader-box loader-three"></div>
    </div>
    <div class="loader-box name-loader"></div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped>
.loader {
  width: 90%;
  max-width: 340px;
  margin: 0 auto 50px;
  overflow: hidden;
  position: relative;
  border-radius: 24px 24px 0 0;
  transition: all 0.3s;

  .loader-box {
    height: 18px;
    background: #ddd;
    border-radius: 6px;
  }

  .loader-image {
    height: 250px;
    background: #ddd;
    border-radius: 24px;
  }

  .other-loaders {
    display: flex;
    justify-content: space-between;
    margin-top: 16px;

    .first-set {
      display: flex;

      .loader-one {
        width: 150px;
        margin-right: 10px;
      }

      .loader-two {
        width: 60px;
      }
    }

    .loader-three {
      width: 40px;
    }
  }

  .name-loader {
    margin-top: 16px;
  }

  &::before {
    content: "";
    display: block;
    position: absolute;
    left: -150px;
    top: 0;
    height: 100%;
    border-radius: 24px;
    width: 150px;
    background: linear-gradient(
      90deg,
      transparent 0%,
      rgba(233, 233, 233, 0.5) 50%,
      transparent 100%
    );
    transition: all 0.3s;
    animation: shimmer 2s cubic-bezier(0.4, 0, 0.2, 1) infinite;
  }

  @media screen and (max-width: 992px) {
    max-width: 320px;
  }

  @media screen and (max-width: 768px) {
    max-width: 320px;
    margin-left: auto;
    margin-right: auto;
  }
}

@keyframes shimmer {
  0% {
    left: -150px;
  }
  100% {
    left: 100%;
  }
}
</style>