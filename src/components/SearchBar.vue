<script setup>
defineProps({
  isActive: {
    type: Boolean,
    default: false,
  },
  searchCity: {
    type: String,
  },
  staysCity: {
    type: Array,
  },
  guests: {
    type: Number,
  },
  childrenGuests: {
    type: Number,
  },
  adultsGuests: {
    type: Number,
  },
  citySearchActive: {
    type: Boolean,
    default: false,
  },
  guestsSearchActive: {
    type: Boolean,
    default: false,
  },
});
const emit = defineEmits([
  "toggleDisplaySearchBar",
  "toggleDisplayCitySearch",
  "toggleDisplayGuestsSearch",
  "changeCitySearch",
  "incrementChildren",
  "decrementChildren",
  "incrementAdults",
  "decrementAdults",
  "filterResults",
]);
function toggleSearchBar() {
  emit("toggleDisplaySearchBar");
}
function toggleCitySearch() {
  emit("toggleDisplayCitySearch");
}
function toggleGuestsSearch() {
  emit("toggleDisplayGuestsSearch");
}
function changeCity(id) {
  emit("changeCitySearch", id);
}
function incrementChildren() {
  emit("incrementChildren");
}
function decrementChildren() {
  emit("decrementChildren");
}
function incrementAdults() {
  emit("incrementAdults");
}
function decrementAdults() {
  emit("decrementAdults");
}
function filterSearch() {
  emit("filterResults");
}
</script>

<template>
  <div :class="['flex', 'column', 'full-sm', isActive ? 'full' : '']">
    <div
      v-if="isActive"
      class="align-center justify-between search-filters-mobile"
    >
      <p class="text-search-mobile">Edit your search</p>
      <div class="flex justify-center align-center">
        <span class="material-icons pointer" @click="toggleSearchBar"
          >close</span
        >
      </div>
    </div>
    <div :class="['search-bar', isActive ? 'active' : '']">
      <!-- City search -->
      <div
        @click="toggleCitySearch"
        :class="[
          'flex',
          'p-16',
          'border-right-grey',
          isActive ? 'label-active col-4 sm-no-border' : '',
          citySearchActive ? 'city-search-active' : '',
        ]"
      >
        <p v-if="isActive" class="label">Location</p>
        <p class="content">{{ searchCity }}</p>
      </div>
      <!-- Guest options -->
      <div
        @click="toggleGuestsSearch"
        :class="[
          'flex',
          'p-16',
          'border-right-grey',
          isActive ? 'label-active col-4 sm-no-border' : '',
          guestsSearchActive ? 'guests-search-active' : '',
        ]"
      >
        <p v-if="isActive" class="label">Guests</p>
        <p class="content guests">{{ !guests ? "Add guests" : guests }}</p>
      </div>

      <!-- Search button  -->
      <div
        :class="[
          'flex',
          'p-16',
          'justify-center',
          'align-center',
          isActive ? 'button-active col-4 search-filters-desktop' : '',
        ]"
      >
        <span
          v-if="!isActive"
          class="material-icons red pointer"
          @click="toggleSearchBar"
          >search</span
        >
        <div
          v-else
          class="btn-search pointer flex align-center"
          @click="filterSearch"
        >
          <span class="material-icons white">search</span>
          <p class="search-text">Search</p>
        </div>
      </div>
    </div>
    <div v-if="isActive" class="search-filters-mobile">
      <div v-if="citySearchActive" :class="['col-4 col-sm-12']">
        <ul>
          <li
            class="flex align-center pointer"
            v-for="city in staysCity"
            :key="city.id"
            @click="changeCity(city.id)"
          >
            <span class="material-icons location-icon">location_on</span>
            <p class="city">{{ city.city }}</p>
          </li>
        </ul>
      </div>
      <div v-if="guestsSearchActive" :class="['col-4 col-sm-12 p-start-40']">
        <div class="flex column mt-36">
          <p class="guests-type m-0">Adults</p>
          <p class="guests-description m-0">Ages 13 or above</p>
          <div class="flex align-center mt-12">
            <div
              class="flex justify-center align-center button-change-guest pointer"
            >
              <p @click="decrementAdults">-</p>
            </div>
            <p class="guest-number">{{ adultsGuests }}</p>
            <div
              class="flex justify-center align-center button-change-guest pointer"
            >
              <p @click="incrementAdults">+</p>
            </div>
          </div>
        </div>
        <div class="flex column mt-36">
          <p class="guests-type m-0">Children</p>
          <p class="guests-description m-0">Ages 2-12</p>
          <div class="flex align-center mt-12">
            <div
              class="flex justify-center align-center button-change-guest pointer"
            >
              <p @click="decrementChildren">-</p>
            </div>
            <p class="guest-number">{{ childrenGuests }}</p>
            <div
              class="flex justify-center align-center button-change-guest pointer"
            >
              <p @click="incrementChildren">+</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div
      :class="[
        'flex',
        'p-16',
        'justify-center',
        'align-center',
        isActive ? 'button-active col-4 search-filters-mobile' : 'd-none',
      ]"
    >
      <span
        v-if="!isActive"
        class="material-icons red pointer"
        @click="toggleSearchBar"
        >search</span
      >
      <div
        v-else
        class="btn-search pointer flex align-center"
        @click="filterSearch"
      >
        <span class="material-icons white">search</span>
        <p class="search-text">Search</p>
      </div>
    </div>
    <div v-if="isActive" class="search-filters-desktop">
      <div :class="['col-4', !citySearchActive ? 'hidden' : '']">
        <ul>
          <li
            class="flex align-center pointer"
            v-for="city in staysCity"
            :key="city.id"
            @click="changeCity(city.id)"
          >
            <span class="material-icons location-icon">location_on</span>
            <p class="city">{{ city.city }}</p>
          </li>
        </ul>
      </div>
      <div :class="['col-4 p-start-40', !guestsSearchActive ? 'hidden' : '']">
        <div class="flex column mt-36">
          <p class="guests-type m-0">Adults</p>
          <p class="guests-description m-0">Ages 13 or above</p>
          <div class="flex align-center mt-12">
            <div
              class="flex justify-center align-center button-change-guest pointer"
            >
              <p @click="decrementAdults">-</p>
            </div>
            <p class="guest-number">{{ adultsGuests }}</p>
            <div
              class="flex justify-center align-center button-change-guest pointer"
            >
              <p @click="incrementAdults">+</p>
            </div>
          </div>
        </div>
        <div class="flex column mt-36">
          <p class="guests-type m-0">Children</p>
          <p class="guests-description m-0">Ages 2-12</p>
          <div class="flex align-center mt-12">
            <div
              class="flex justify-center align-center button-change-guest pointer"
            >
              <p @click="decrementChildren">-</p>
            </div>
            <p class="guest-number">{{ childrenGuests }}</p>
            <div
              class="flex justify-center align-center button-change-guest pointer"
            >
              <p @click="incrementChildren">+</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.text-search-mobile {
  font-style: normal;
  font-weight: 700;
  font-size: 12px;
  color: #333333;
}
.hidden {
  visibility: hidden;
}
li:hover {
  transform: scale(1.04);
}
.p-start-40 {
  padding-left: 40px;
}
.mt-12 {
  margin-top: 12px;
}
.guest-number {
  margin: 0 16px;
  color: #333333;
  font-size: 14px;
  font-weight: 700;
}
.button-change-guest {
  width: 23px;
  height: 23px;
  border: 1px solid #828282;
  border-radius: 4px;
}
.button-change-guest > p {
  color: #828282;
  padding: 7px;
}
.text-center {
  text-align: center;
}
.guests-type {
  color: #333333;
  font-size: 14px;
  font-weight: 700;
}
.guests-description {
  color: #bdbdbd;
  font-size: 14px;
  font-weight: 700;
  margin-top: 4px;
}
.location-icon {
  color: #4f4f4f;
}

.m-0 {
  margin: 0;
}

.mt-36 {
  margin-top: 36px;
}

li {
  list-style: none;
  margin-top: 36px;
}
.city {
  margin: 0 0 0 10px;
  font-size: 14px;
  font-weight: 400;
}
.col-4 {
  width: calc(100% / 3);
}
.label-active {
  flex-direction: column;
}

.btn-search {
  background-color: #eb5757;
  box-shadow: 0px 1px 6px rgba(0, 0, 0, 0.1);
  border-radius: 16px;
  border: none;
  padding: 15px 25px;
}
.btn-search:hover,
.btn-search:focus {
  outline: none;
}
.search-text {
  color: #ffffff;
  font-size: 14px;
  font-weight: 700;
  margin: 0;
}
span.material-icons.red {
  color: #eb5757;
}
span.material-icons.white {
  color: #ffffff;
}
p {
  color: #333333;
}
.guests {
  color: #bdbdbd;
}
.content {
  font-size: 14px;
  font-weight: 400;
  margin: 0;
}
.label {
  font-weight: 800;
  font-size: 9px;
  text-transform: uppercase;
}
.p-16 {
  padding: 16px;
}
.border-right-grey {
  border-right: 1px solid #f2f2f2;
}
.search-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-radius: 16px;
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.1);
  width: auto;
}
.search-filters-desktop {
  display: flex;
}

@media screen and (max-width: 575px) {
  .city-search-active,
  .guests-search-active,
  .label-active:hover {
    outline: none;
    border: none;
    filter: drop-shadow(0px 1px 6px rgba(0, 0, 0, 0.1));
    border-radius: 16px;
  }
  .col-sm-12 {
    width: 100% !important;
  }
  .search-bar {
    margin-top: 39px;
    align-self: center;
    align-items: flex-start;
  }
  .search-bar.active {
    width: 100%;
    margin-top: 0 !important;
    flex-direction: column;
  }

  .button-active {
    align-self: center;
  }
  .full-sm {
    width: 100%;
  }

  .search-filters-desktop {
    display: none;
  }
  .search-filters-mobile {
    display: flex;
  }

  .sm-no-border:first-child {
    border-bottom: 1px solid #f2f2f2;
  }

  .sm-no-border {
    border: none;
    width: 100% !important;
  }
}

@media screen and (min-width: 576px) {
  .search-filters-mobile {
    display: none;
  }
  .search-filters-desktop {
    display: flex;
  }

  .city-search-active,
  .guests-search-active,
  .label-active:hover {
    outline: none;
    border: 1px solid #333333 !important;
    filter: drop-shadow(0px 1px 6px rgba(0, 0, 0, 0.1));
    border-radius: 16px;
  }
}
</style>
