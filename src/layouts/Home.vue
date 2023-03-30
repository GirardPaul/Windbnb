<script setup>
import SearchBar from "../components/SearchBar.vue";
import Card from "../components/Card.vue";
import staysRef from "../assets/stays.json";
import { onMounted, onBeforeMount } from "vue";
import { ref } from "vue";

const stays = ref([]),
  searchBarIsActive = ref(false),
  guests = ref(0),
  searchCity = ref("Helsinki, Finland"),
  staysCity = ref([]);

onBeforeMount(() => {
  stays.value = staysRef;
  staysCity.value = stays.value.map((stay) => {
    return `${stay.city}, ${stay.country}`;
  });
});

onMounted(() => {
  const city = searchCity.value.split(",")[0];
  const country = searchCity.value.split(",")[1].trim();
  stays.value = stays.value.filter(
    (stay) => stay.city === city && stay.country === country
  );
});
</script>
<template>
  <header>
    <img src="../assets/logo.png" alt="windbnb-logo" />
    <SearchBar
      :searchCity="searchCity"
      :staysCity="staysCity"
      :isActive="searchBarIsActive"
      :guests="guests"
    />
  </header>
  <main>
    <div class="flex justify-between align-center">
      <h3>Stays in Finland</h3>
      <p class="stays">{{ stays.length }}+ stays</p>
    </div>
    <div class="grid">
      <div class="card" v-for="stay in stays" :key="stay.id">
        <Card :stay="stay" />
      </div>
    </div>
  </main>
</template>
<style scoped>
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 24px;
  margin-top: 32px;
}
main {
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

  .search-bar {
    margin-top: 39px;
    align-self: center;
  }
}

@media screen and (max-width: 470px) {
  .grid {
    grid-template-columns: repeat(1, 1fr);
  }
}

@media screen and (min-width: 471px) and (max-width: 767px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
