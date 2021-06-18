<template>
  <container>
    <!-- NAVBAR -->
    <Navbar
      :guests="guests"
      :location="location"
      @open-search-modal="openSearchModal = true"
    />

    <!-- HEADER SECTION -->
    <ApartmentsHeader :length="filteredApartments.length" />

    <!-- LOADING ANIMATIONS -->
    <flex-container v-if="isLoading">
      <apartment-loader
        v-for="apartment in 9"
        :key="apartment"
      ></apartment-loader>
    </flex-container>

    <!-- LIST OF AVAILABLE APARTMENTS -->
    <flex-container
      v-else-if="
        !isLoading && filteredApartments && filteredApartments.length > 0
      "
    >
      <single-apartment
        v-for="apartment in filteredApartments"
        :key="apartment.title"
        :apartment="apartment"
      ></single-apartment>
    </flex-container>

    <EmptySearch @clear-search="clearSearch" v-else />

    <!-- SEARCH HOTELS MODAL -->
    <teleport to="#app">
      <SearchModal
        :location="location"
        :guests="guests"
        :locations="locations"
        :showLocation="showLocation"
        :showGuest="showGuest"
        :numberOfChildren="numberOfChildren"
        :numberOfAdults="numberOfAdults"
        v-if="openSearchModal"
        @set-location="setLocation"
        @clear-location="clearLocation"
        @show-locations="showLocations"
        @show-guests="showGuests"
        @close-search-modal="openSearchModal = false"
        @increment-children="incrementChildren"
        @decrement-children="decrementChildren"
        @increment-adults="incrementAdults"
        @decrement-adults="decrementAdults"
      />
    </teleport>
  </container>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import SingleApartment from "./components/SingleApartment.vue";
import ApartmentLoader from "./components/ApartmentLoader.vue";
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
import EmptySearch from "./components/EmptySearch.vue";
import SearchModal from "./components/SearchModal.vue";
import FlexContainer from "./components/FlexContainer.vue";
import Container from "./components/Container.vue";
import ApartmentsHeader from "./components/ApartmentsHeader.vue";

export default {
  name: "App",
  components: {
    Navbar,
    Footer,
    EmptySearch,
    SingleApartment,
    ApartmentLoader,
    SearchModal,
    FlexContainer,
    Container,
    ApartmentsHeader,
  },
  setup() {
    // Data
    const isLoading = ref(true);
    const location = ref(null);
    const guests = ref(null);
    const openSearchModal = ref(false);
    const showLocation = ref(false);
    const showGuest = ref(false);
    const numberOfAdults = ref(0);
    const numberOfChildren = ref(0);
    const apartments = ref([
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
    ]);
    const locations = ref(["Helsinki", "Turku", "Oulu", "Vaasa"]);

    // On Mounted LIfecylce Hook
    onMounted(() => {
      setTimeout(() => {
        isLoading.value = false;
      }, 1500);
    });

    // Methods
    const filterByGuests = (apartments) => {
      if (guests.value) {
        return apartments.filter(
          (apartment) => apartment.maxGuests >= guests.value
        );
      }
      return apartments;
    };

    const filterByLocation = (apartments) => {
      if (location.value) {
        return apartments.filter(
          (apartment) => apartment.city === location.value
        );
      }
      return apartments;
    };

    const clearSearch = () => {
      location.value = null;
      guests.value = null;
      numberOfAdults.value = 0;
      numberOfChildren.value = 0;
    };

    const showLocations = () => {
      showLocation.value = true;
      showGuest.value = false;
    };

    const showGuests = () => {
      showGuest.value = true;
      showLocation.value = false;
    };

    const clearLocation = () => {
      location.value = null;
    };

    const setLocation = (value) => {
      location.value = value;
    };

    const decrementAdults = () => {
      numberOfAdults.value--;
      if (guests.value !== null || guests.value !== 0) {
        guests.value--;
      }
    };

    const incrementAdults = () => {
      numberOfAdults.value++;
      guests.value++;
    };

    const decrementChildren = () => {
      numberOfChildren.value--;
      if (guests.value !== null || guests.value !== 0) {
        guests.value--;
      }
    };

    const incrementChildren = () => {
      numberOfChildren.value++;
      guests.value++;
    };

    // Computed Properties
    const filteredApartments = computed(() => {
      return filterByGuests(filterByLocation(apartments.value));
    });

    return {
      isLoading,
      location,
      guests,
      openSearchModal,
      showLocation,
      showGuest,
      numberOfAdults,
      numberOfChildren,
      apartments,
      locations,
      filteredApartments,
      clearSearch,
      showLocations,
      showGuests,
      clearLocation,
      setLocation,
      decrementAdults,
      incrementAdults,
      decrementChildren,
      incrementChildren
    };
  },
};
</script>

<style lang="scss">
// GOOGLE FONTS
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Mulish:wght@300;400;500;600;700&display=swap");

// RESETTING BROWSER DEFAULT STYLINGS
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

// SETTING FONT FAMILY
#app {
  font-family: "Montserrat", sans-serif;
}
</style>
