<template>
  <div class="app-content">
    <header>
      <div class="logo">
        <div class="triangle"></div>
        <h6>windbnb</h6>
      </div>
      <div class="search-form" @click="openSearchModal = true">
        <div class="location">
          <span v-if="location">{{ location }}, Finland</span>
          <span class="empty" v-else>Choose a Location</span>
        </div>
        <div class="guests">
          <span v-if="guests"
            >{{ guests }} guest<span v-if="guests > 1">s</span></span
          >
          <span class="empty" v-else>Add guests</span>
        </div>
        <div class="search">
          <i class="fa fa-search"></i>
        </div>
      </div>
    </header>
    <div class="available-apartments">
      <header class="header">
        <h3>Stays in Finland</h3>
        <p>{{ filteredApartments.length }}+ stays</p>
      </header>
      <div class="loading-section" v-if="isLoading">
        <apartment-loader
          v-for="apartment in 9"
          :key="apartment"
        ></apartment-loader>
      </div>
      <div class="all-apartments" v-else>
        <single-apartment
          v-for="apartment in filteredApartments"
          :key="apartment.title"
          :apartment="apartment"
        ></single-apartment>
      </div>
    </div>
    <teleport to="#app">
      <div
        class="search-modal"
        @click.self="openSearchModal = false"
        v-if="openSearchModal"
      >
        <div class="modal">
          <div class="search-form">
            <div class="location">
              <p>Location</p>
              <span v-if="location">{{ location }}, Finland</span>
              <span class="empty" v-else>Choose a Location</span>
              <button class="clear" @click="clearLocation" v-if="location">
                <i class="fa fa-times"></i>
              </button>
            </div>
            <div class="guests">
              <p>guests</p>
              <span v-if="guests"
                >{{ guests }} guest<span v-if="guests > 1">s</span></span
              >
              <span class="empty" v-else>Add guests</span>
            </div>
            <div class="submit">
              <button><i class="fa fa-search"></i> Search</button>
            </div>
          </div>
          <div class="search-options">
            <div class="location">
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
            <div class="guests">
              <div class="adults">
                <h6>Adults</h6>
                <span>Age 13 and above</span>
                <div class="adults-counter">
                  <button
                    @click="decrementAdults"
                    :disabled="numberOfAdults <= 0"
                  >
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
    </teleport>
  </div>
</template>

<script>
import SingleApartment from "./components/SingleApartment.vue";
import ApartmentLoader from "./components/ApartmentLoader.vue";

export default {
  name: "App",
  components: { SingleApartment, ApartmentLoader },
  computed: {
    filteredApartments() {
      return this.filterByGuests(this.filterByLocation(this.apartments));
    },
  },
  data() {
    return {
      isLoading: true,
      location: null,
      guests: null,
      openSearchModal: false,
      numberOfAdults: 0,
      numberOfChildren: 0,
      apartments: [
        {
          city: "Helsinki",
          country: "Finland",
          superHost: false,
          title: "Stylist apartment in center of the city",
          rating: 4.4,
          maxGuests: 3,
          type: "Entire apartment",
          beds: 2,
          photo:
            "https://images.unsplash.com/photo-1505873242700-f289a29e1e0f?ixlib=rb-1.2.1&auto=format&fit=crop&w=2255&q=80",
        },
        {
          city: "Turku",
          country: "Finland",
          superHost: false,
          title: "Nice apartment in center of Helsinki",
          rating: 4.2,
          maxGuests: 5,
          type: "Entire apartment",
          beds: 3,
          photo:
            "https://images.unsplash.com/photo-1554995207-c18c203602cb?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80",
        },
        {
          city: "Helsinki",
          country: "Finland",
          superHost: true,
          title: "Arty interior in 1900 wooden house",
          rating: 4.5,
          maxGuests: 10,
          type: "Entire house",
          beds: 6,
          photo:
            "https://images.unsplash.com/photo-1505691938895-1758d7feb511?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80",
        },
        {
          city: "Helsinki",
          country: "Finland",
          superHost: false,
          title: "Apartment next to market spuare",
          rating: 4.48,
          maxGuests: 3,
          type: "Entire apartment",
          beds: null,
          photo:
            "https://images.unsplash.com/photo-1556020685-ae41abfc9365?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=934&q=80",
        },
        {
          city: "Turku",
          country: "Finland",
          superHost: true,
          title: "Villa Aurora guest-house",
          rating: 4.75,
          maxGuests: 9,
          type: "Entire house",
          beds: null,
          photo:
            "https://images.unsplash.com/photo-1513694203232-719a280e022f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2249&q=80",
        },
        {
          city: "Vaasa",
          country: "Finland",
          superHost: true,
          title: "A cosy family house",
          rating: 4.95,
          maxGuests: 6,
          type: "Entire house",
          beds: null,
          photo:
            "https://images.unsplash.com/photo-1524758631624-e2822e304c36?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80",
        },
        {
          city: "Vaasa",
          country: "Finland",
          superHost: false,
          title: "Lovely Studio near Railway Station",
          rating: 4.68,
          maxGuests: 2,
          type: "Private room",
          beds: null,
          photo:
            "https://images.unsplash.com/photo-1505693314120-0d443867891c?ixlib=rb-1.2.1&auto=format&fit=crop&w=2591&q=80",
        },
        {
          city: "Oulu",
          country: "Finland",
          superHost: false,
          title: "Peaceful little home in city center",
          rating: 4.12,
          maxGuests: 6,
          type: "Entire house",
          beds: 3,
          photo:
            "https://images.unsplash.com/photo-1522708323590-d24dbb6b0267?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80",
        },
        {
          city: "Oulu",
          country: "Finland",
          superHost: false,
          title: "Beautiful new studio apartment nearby the center",
          rating: 4.49,
          maxGuests: 2,
          type: "Entire apartment",
          beds: 1,
          photo:
            "https://images.unsplash.com/photo-1507089947368-19c1da9775ae?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2255&q=80",
        },
        {
          city: "Oulu",
          country: "Finland",
          superHost: true,
          title: "Cozy woodhouse flat with wooden sauna",
          rating: 4.38,
          maxGuests: 4,
          type: "Entire house",
          beds: null,
          photo:
            "https://images.unsplash.com/photo-1522156373667-4c7234bbd804?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjF9&auto=format&fit=crop&w=930&q=80",
        },
        {
          city: "Vaasa",
          country: "Finland",
          superHost: false,
          title: "Brand new studio apartment near the harbour",
          rating: 4.89,
          maxGuests: 6,
          type: "Entire apartment",
          beds: 3,
          photo:
            "https://images.unsplash.com/photo-1494203484021-3c454daf695d?ixlib=rb-1.2.1&auto=format&fit=crop&w=2250&q=80",
        },
        {
          city: "Helsinki",
          country: "Finland",
          superHost: false,
          title: "Beautiful and comfortable old wooden house",
          rating: 4.63,
          maxGuests: 10,
          type: "Entire house",
          beds: null,
          photo:
            "https://images.unsplash.com/photo-1516455590571-18256e5bb9ff?ixlib=rb-1.2.1&auto=format&fit=crop&w=2250&q=80",
        },
        {
          city: "Turku",
          country: "Finland",
          superHost: false,
          title: "Turku Nordic Home near city center",
          rating: 4.24,
          maxGuests: 5,
          type: "Entire apartment",
          beds: 3,
          photo:
            "https://images.unsplash.com/photo-1519643381401-22c77e60520e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjE3MzYxfQ&auto=format&fit=crop&w=2253&q=80",
        },
        {
          city: "Turku",
          country: "Finland",
          superHost: true,
          title: "Nice 2 room apartment close to everything",
          rating: 4.34,
          maxGuests: 6,
          type: "Entire apartment",
          beds: 3,
          photo:
            "https://images.unsplash.com/photo-1523755231516-e43fd2e8dca5?ixlib=rb-1.2.1&auto=format&fit=crop&w=1275&q=80",
        },
      ],
      locations: ["Helsinki", "Turku", "Oulu", "Vaasa"],
    };
  },
  methods: {
    filterByGuests(apartments) {
      if (this.guests) {
        return apartments.filter(
          (apartment) => apartment.maxGuests >= this.guests
        );
      }
      return apartments;
    },
    filterByLocation(apartments) {
      if (this.location) {
        return apartments.filter(
          (apartment) => apartment.city === this.location
        );
      }
      return apartments;
    },
    clearLocation() {
      this.location = null;
    },
    setLocation(location) {
      this.location = location;
    },
    decrementAdults() {
      this.numberOfAdults--;
      if (this.guests !== null || this.guests !== 0) {
        this.guests--;
      }
    },
    incrementAdults() {
      this.numberOfAdults++;
      this.guests++;
    },
    decrementChildren() {
      this.numberOfChildren--;
      if (this.guests !== null || this.guests !== 0) {
        this.guests--;
      }
    },
    incrementChildren() {
      this.numberOfChildren++;
      this.guests++;
    },
  },
  mounted() {
    setTimeout(() => {
      this.isLoading = false;
    }, 1500)
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Mulish:wght@300;400;500;600;700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: "Montserrat", sans-serif;
}

.app-content {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-left: auto;
  margin-right: auto;

  @media (min-width: 576px) {
    max-width: 540px;
  }

  @media (min-width: 768px) {
    max-width: 720px;
  }

  @media (min-width: 992px) {
    max-width: 960px;
  }

  @media (min-width: 1200px) {
    max-width: 1140px;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
    margin-bottom: 50px;

    .logo {
      display: flex;

      .triangle {
        width: 0;
        height: 0;
        border-left: 10px solid transparent;
        border-right: 10px solid transparent;
        border-bottom: 16px solid #eb5757;
      }

      h6 {
        font-family: "Poppins", sans-serif;
        font-style: normal;
        font-weight: bold;
        font-size: 14px;
        line-height: 21px;
        color: #eb5757;
        padding-left: 6px;
      }
    }

    .search-form {
      background: #ffffff;
      box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
      border-radius: 16px;
      display: flex;
      align-items: center;
      width: max-content;
      cursor: pointer;

      .location,
      .guests {
        position: relative;
        padding: 20px 20px;
        border-right: 1px solid #eee;
        font-family: "Mulish", sans-serif;

        span {
          font-size: 14px;
          line-height: 18px;
        }

        .empty {
          color: #bdbdbd;
        }
      }

      .search {
        padding: 20px 20px;

        i {
          color: #eb5757;
        }
      }
    }
  }

  .available-apartments {
    header {
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

    .loading-section,
    .all-apartments {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      width: 100%;
    }
  }
}

.search-modal {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
  background: rgba(79, 79, 79, 0.4);
  z-index: 9999;

  .modal {
    background: #fff;
    width: 100%;
    height: 60vh;
    padding: 50px 100px;

    .search-form {
      background: #ffffff;
      box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
      border-radius: 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;

      .location,
      .guests {
        position: relative;
        flex: 1 1 33.3%;
        width: 33.3%;
        padding: 10px 20px;
        border-right: 1px solid #eee;

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

      .submit {
        flex: 1 1 33.3%;
        width: 33.3%;

        button {
          padding: 12px 20px;
          display: block;
          margin: auto;
          background: #eb5757;
          color: #fff;
          outline: none;
          border: none;
          box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
          border-radius: 16px;
        }
      }
    }

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
      }

      .location {
        padding-left: 30px;

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
      }

      .guests {
        font-family: "Mulish";

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
