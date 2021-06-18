<template>
  <div class="search-modal">
    <div class="modal">
      <header>
        <h5>Edit your search</h5>
        <button class="close" @click="closeSearchModal">
          <i class="fa fa-times"></i>
        </button>
      </header>
      <div class="search-form">
        <div
          class="location"
          :class="{ active: showLocation }"
          @click="$emit('show-locations')"
        >
          <p>Location</p>
          <span v-if="location">{{ location }}, Finland</span>
          <span class="empty" v-else>Choose a Location</span>
          <button class="clear" @click="$emit('clear-location')" v-if="location">
            <i class="fa fa-times"></i>
          </button>
        </div>
        <div class="guests" :class="{ active: showGuest }" @click="$emit('show-guests')">
          <p>guests</p>
          <span v-if="guests"
            >{{ guests }} guest<span v-if="guests > 1">s</span></span
          >
          <span class="empty" v-else>Add guests</span>
        </div>
        <div class="submit">
          <button @click="closeSearchModal">
            <i class="fa fa-search"></i> Search
          </button>
        </div>
      </div>
      <div class="search-options">
        <div class="location" :class="{ invisible: !showLocation }">
          <ul>
            <li
              v-for="location in locations"
              :key="location"
              @click="setLocation(location)"
            >
              <i class="fa fa-map-marker-alt"></i> {{ location }}, Finland
            </li>
          </ul>
        </div>
        <div class="guests" :class="{ invisible: !showGuest }">
          <div class="adults">
            <h6>Adults</h6>
            <span>Age 13 and above</span>
            <div class="adults-counter">
              <button @click="decrementAdults" :disabled="numberOfAdults <= 0">
                -
              </button>
              {{ numberOfAdults }}
              <button @click="incrementAdults">+</button>
            </div>
          </div>
          <div class="children">
            <h6>Children</h6>
            <span>Ages 2 - 12</span>
            <div class="children-counter">
              <button
                @click="decrementChildren"
                :disabled="numberOfChildren <= 0"
              >
                -
              </button>
              {{ numberOfChildren }}
              <button @click="incrementChildren">+</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchModal",
  props: ['location', 'guests', 'numberOfAdults', 'numberOfChildren', 'locations', 'showLocation', 'showGuest'],
  emits: ['show-locations', 'show-guests', 'close-search-modal', 'set-location', 'clear-location', 'increment-children', 'decrement-chilren', 'increment-adults', 'decrement-adults'],
  setup(_, context) {
    const closeSearchModal = () => {
      context.emit('close-search-modal');
    }
    const setLocation = (location) => {
      context.emit('set-location', location)
    }
    const incrementChildren = () => {
      context.emit('increment-children')
    }
    const decrementChildren = () => {
      context.emit('decrement-children')
    }
    const incrementAdults = () => {
      context.emit('increment-adults')
    }
    const decrementAdults = () => {
      context.emit('decrement-adults')
    }
    return { closeSearchModal, setLocation, incrementChildren, decrementChildren, incrementAdults, decrementAdults}
  }
};
</script>

<style lang="scss">
// SEARCH FORM MODAL
.search-modal {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  background: rgba(79, 79, 79, 0.4);
  z-index: 9999;

  // SEARCH FORM INNER MODAL
  .modal {
    background: #fff;
    width: 100%;
    height: 60vh;
    padding: 50px 100px;
    position: relative;

    @media screen and (max-width: 800px) {
      padding: 50px;
    }

    @media screen and (max-width: 660px) {
      padding: 50px 30px;
    }

    @media screen and (max-width: 600px) {
      height: 80vh;
    }

    // SEARCH FORM HEADER
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 30px;

      @media screen and (min-width: 660px) {
        display: none;
      }

      h5 {
        font-weight: bold;
        font-size: 14px;
        line-height: 15px;
        color: #333;
      }

      .close {
        border: none;
        background: none;
        font-size: 18px;
        color: #333;
        cursor: pointer;

        &:focus {
          outline: none;
        }
      }
    }

    // SEARCH MODAL FORM
    .search-form {
      background: #ffffff;
      box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
      border-radius: 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;

      @media screen and (max-width: 414px) {
        flex-direction: column;
        align-items: flex-start;
      }

      .location,
      .guests {
        position: relative;
        flex: 1 1 33.3%;
        width: 33.3%;
        padding: 10px 20px;
        border-right: 1px solid #eee;
        border-radius: 0;
        cursor: pointer;

        &.active {
          border: 1px solid #333;
          border-radius: 20px;
        }

        @media screen and (max-width: 600px) {
          flex: 1 1 50%;
          width: 50%;
        }

        @media screen and (max-width: 414px) {
          border-right: none;
          border-bottom: 1px solid #eee;
          flex: 1 1 100%;
          width: 100%;
        }

        p {
          font-family: "Mulish", sans-serif;
          font-weight: bold;
          font-size: 10px;
          line-height: 11px;
          text-transform: uppercase;
          color: #333333;
          margin-bottom: 5px;
        }

        span {
          font-size: 14px;
          line-height: 18px;
        }

        .empty {
          color: #bdbdbd;
        }

        .clear {
          position: absolute;
          border: none;
          background: none;
          top: 15px;
          right: 15px;
          font-size: 20px;
          font-weight: 700;
          cursor: pointer;

          &:focus {
            outline: none;
          }

          i {
            color: #aaa;
          }
        }
      }

      .guests {
        border-bottom: none;
      }

      .submit {
        flex: 1 1 33.3%;
        width: 33.3%;

        @media screen and (max-width: 600px) {
          position: absolute;
          bottom: 30px;
          left: 50%;
          transform: translateX(-50%);
          flex: 1 1 100%;
          width: 100%;
        }

        button {
          padding: 12px 20px;
          display: block;
          margin: auto;
          background: #eb5757;
          color: #f2f2f2;
          border: none;
          box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
          border-radius: 16px;
          font-family: "Mulish", sans-serif;
          font-weight: bold;
          font-size: 14px;
          line-height: 18px;
          cursor: pointer;

          &:focus {
            outline: none;
          }

          i {
            margin-right: 4px;
          }
        }
      }
    }

    // SEARCH MODAL OPTIONS TO CHOOSE FROM
    .search-options {
      margin-top: 40px;
      display: flex;
      justify-content: flex-start;
      align-items: flex-start;

      .location,
      .guests {
        flex: 1 1 33.3%;
        width: 33.3%;
        max-width: 33.3%;

        @media screen and (max-width: 600px) {
          flex: 1 1 50%;
          width: 50%;
          max-width: 50%;
        }
      }

      .location {
        padding-left: 15px;
        visibility: visible;

        ul {
          list-style: none;

          li {
            width: max-content;
            margin-bottom: 25px;
            font-family: "Mulish", sans-serif;
            font-size: 14px;
            line-height: 18px;
            color: #4f4f4f;
            cursor: pointer;

            i {
              margin-right: 6px;
              color: #4f4f4f;
              font-size: 18px;
            }
          }
        }

        &.invisible {
          visibility: hidden;

          @media screen and (max-width: 414px) {
            display: none;
          }
        }
      }

      .guests {
        visibility: visible;
        padding-left: 15px;
        font-family: "Mulish";

        &.invisible {
          visibility: hidden;
        }

        .adults,
        .children {
          h6 {
            font-weight: bold;
            font-size: 14px;
            line-height: 10px;
            color: #333333;
          }

          span {
            display: inline-block;
            color: #bdbdbd;
            font-size: 12px;
            margin-bottom: 10px;
          }

          .children-counter,
          .adults-counter {
            button {
              background: none;
              border: 1px solid #828282;
              border-radius: 4px;
              padding: 3px 8px;
              cursor: pointer;
              outline: none;

              &:nth-child(1) {
                margin-right: 8px;
              }

              &:nth-child(2) {
                margin-left: 8px;
              }
            }
          }
        }

        .adults {
          margin-bottom: 30px;
        }
      }
    }
  }
}
</style>