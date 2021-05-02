<template>
  <div class="single-apartment">
    <div class="apartment-image">
      <img :src="apartment.photo" alt="Apartment Image" />
    </div>
    <div class="apartment-details">
      <div class="type-and-rating">
        <div class="type">
          <span class="super-host" v-if="apartment.superHost">super host</span>
          <span class="type-name">{{ apartment.type }}</span>
          <span class="beds" v-if="apartment.beds >= 2">{{ apartment.beds }} beds</span>
        </div>
        <div class="rating">
          <i class="fa fa-star"></i>
          <span>{{ apartment.rating }}</span>
        </div>
      </div>
      <h3 class="title">{{ apartment.title }}</h3>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    apartment: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.single-apartment {
  width: 90%;
  max-width: 340px;
  margin-bottom: 50px;

  .apartment-image {
    margin-bottom: 14px;

    img {
      height: 250px;
      width: 100%;
      max-width: 100%;
      border-radius: 24px;
      object-fit: cover;
    }
  }

  .apartment-details {
    .type-and-rating {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      padding: 4px 0;
      min-height: 33px;

      .type {
        display: flex;
        align-items: center;
        justify-content: flex-start;

        .super-host {
          font-weight: bold;
          font-size: 10px;
          line-height: 15px;
          text-transform: uppercase;
          color: #4f4f4f;
          padding: 4px 6px;
          margin-right: 5px;
          border: 1px solid #4f4f4f;
          border-radius: 12px;

          @media screen and (max-width: 384px) {
            font-size: 6px;
          }
        }

        .beds,
        .type-name {
          font-size: 14px;
          line-height: 17px;
          color: #828282;
        }

        .beds {
          padding-left: 15px;
          position: relative;

          &::before {
            content: "";
            position: absolute;
            height: 2px;
            width: 2px;
            border-radius: 50%;
            bottom: 4px;
            left: 7px;
            background-color: #828282;
          }
        }
      }

      .rating {
        display: flex;
        justify-content: center;
        align-items: center;

        span {
          font-size: 13px;
          color: #4f4f4f;
        }

        i {
          margin-right: 6px;
          font-size: 14px;
          color: #eb5757;
          line-height: 0;
        }
      }
    }
    .title {
      font-weight: 600;
      font-size: 16px;
      line-height: 20px;
      color: #333333;
      display: -webkit-box;
      -webkit-line-clamp: 1;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }
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
</style>