<script setup>
import SearchBar from "../components/SearchBar.vue";
import Card from "../components/Card.vue";
import staysRef from "../assets/stays.json";
import { onMounted, onBeforeMount } from "vue";
import { ref, computed } from "vue";

const stays = ref([]),
  searchBarIsActive = ref(false),
  citySearchActive = ref(false),
  guestsSearchActive = ref(false),
  childrenGuests = ref(0),
  adultsGuests = ref(0),
  searchCity = ref("Helsinki, Finland"),
  staysCity = ref([]);

onBeforeMount(() => {
  stays.value = staysRef;
  staysCity.value = stays.value
    .map((stay) => {
      return {
        id: stay.id,
        city: `${stay.city}, ${stay.country}`,
      };
    })
    .reduce((acc, curr) => {
      const x = acc.find((item) => item.city === curr.city);
      if (!x) {
        return acc.concat([curr]);
      } else {
        return acc;
      }
    }, []);
});

onMounted(() => {
  const city = searchCity.value.split(",")[0];
  const country = searchCity.value.split(",")[1].trim();
  stays.value = stays.value.filter(
    (stay) => stay.city === city && stay.country === country
  );
});

function closeSearchBar() {
  if (searchBarIsActive.value) {
    toggleOffActive();
    filterResults();
  }
}

function toggleOffActive() {
  searchBarIsActive.value = false;
  citySearchActive.value = false;
  guestsSearchActive.value = false;
}

function toggleCitySearch() {
  if (!searchBarIsActive.value) {
    searchBarIsActive.value = true;
  }
  citySearchActive.value = !citySearchActive.value;
  guestsSearchActive.value = false;
}

function toggleGuestsSearch() {
  if (!searchBarIsActive.value) {
    searchBarIsActive.value = true;
  }
  guestsSearchActive.value = !guestsSearchActive.value;
  citySearchActive.value = false;
}

function changeCity(id) {
  const retreiveCity = staysRef.find((stay) => stay.id === id);
  searchCity.value = `${retreiveCity.city}, ${retreiveCity.country}`;
}

function toggleDisplaySearchBar() {
  searchBarIsActive.value = !searchBarIsActive.value;
  citySearchActive.value = false;
  guestsSearchActive.value = false;
}

function filterResults() {
  const city = searchCity.value.split(",")[0];
  const country = searchCity.value.split(",")[1].trim();

  let allStays = [...staysRef];

  if (city && country) {
    allStays = allStays.filter(
      (stay) => stay.city === city && stay.country === country
    );
  }

  if (guests.value) {
    allStays = allStays.filter((stay) => stay.maxGuests >= guests.value);
  }

  stays.value = allStays;
  toggleOffActive();
}

const guests = computed(() => {
  return childrenGuests.value + adultsGuests.value;
});
</script>
<template>
  <header :class="[searchBarIsActive ? 'header-active' : '']">
    <img
      v-if="!searchBarIsActive"
      src="../assets/logo.png"
      alt="windbnb-logo"
    />
    <SearchBar
      :citySearchActive="citySearchActive"
      :guestsSearchActive="guestsSearchActive"
      :searchCity="searchCity"
      :staysCity="staysCity"
      :isActive="searchBarIsActive"
      :guests="guests"
      :childrenGuests="childrenGuests"
      :adultsGuests="adultsGuests"
      @toggleDisplaySearchBar="toggleDisplaySearchBar"
      @toggleDisplayCitySearch="toggleCitySearch"
      @toggleDisplayGuestsSearch="toggleGuestsSearch"
      @changeCitySearch="changeCity"
      @incrementChildren="childrenGuests++"
      @decrementChildren="childrenGuests && childrenGuests--"
      @incrementAdults="adultsGuests++"
      @decrementAdults="adultsGuests && adultsGuests--"
      @filterResults="filterResults"
    />
  </header>
  <section
    @click="closeSearchBar"
    :class="[searchBarIsActive ? 'section-inactive pointer' : '']"
  >
    <div class="flex justify-between align-center">
      <h3>Stays in Finland</h3>
      <p class="stays">{{ stays.length }}+ stays</p>
    </div>
    <div class="grid">
      <div class="card" v-for="stay in stays" :key="stay.id">
        <Card :stay="stay" />
      </div>
    </div>
  </section>
  <footer>
    <p>Created by Paul Girard - devChallenges.io</p>
  </footer>
</template>
<style scoped>
footer {
  display: flex;
  margin-top: 26px;
  justify-content: center;
  align-items: center;
}

footer > p {
  margin: 0;
  font-size: 14px;
  color: #828282;
  font-weight: 500;
}
header.header-active {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1;
  background-color: #fff;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1);
  padding: 24px 24px 0;
  height: 460px;
  align-items: baseline !important;
  padding: 93px 96px;
}

header.header-active > .search-bar {
  width: 100% !important;
}

section.section-inactive {
  opacity: 0.4;
  margin-top: 0;
}
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 24px;
  margin-top: 32px;
}
section {
  margin-top: 85px;
}
h3 {
  font-weight: 700;
  font-size: 24px;
  color: #333333;
  margin: 0;
}

p {
  margin: 0;
}

p.stays {
  color: #4f4f4f;
  font-size: 14px;
  font-weight: 500;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

@media screen and (max-width: 575px) {
  header {
    flex-direction: column;
    align-items: flex-start;
  }

  header.header-active {
    height: 100vh !important;

    padding: 12px !important;
  }
}

@media screen and (max-width: 470px) {
  .grid {
    grid-template-columns: repeat(1, 1fr);
  }
}

@media screen and (min-width: 471px) and (max-width: 1024px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
