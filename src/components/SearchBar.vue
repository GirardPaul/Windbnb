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
});
</script>

<template>
  <div class="search-bar">
    <!-- City search -->
    <div class="flex p-16 border-right-grey">
      <p v-show="isActive" class="label">Location</p>
      <p class="content">{{ searchCity }}</p>
    </div>
    <!-- Guest options -->
    <div class="flex p-16 border-right-grey">
      <p v-show="isActive" class="label">Guests</p>
      <p class="content guests">Add guests</p>
    </div>
    <!-- Search button  -->
    <div class="flex p-16 justify-center align-center">
      <span
        v-if="!isActive"
        class="material-icons red"
        @click="console.log('open modal')"
        >search</span
      >
      <button
        type="button"
        v-else
        class="btn-search"
        @click="console.log('search')"
      >
        <template>
          <span class="material-icons white">search</span>
          <p class="search-text">Search</p>
        </template>
      </button>
    </div>
  </div>
</template>

<style scoped>
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
}
span.material-icons.red {
  color: #eb5757;
  cursor: pointer;
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
}
.label {
  font-weight: 800;
  font-size: 9px;
  text-transform: uppercase;
  margin-bottom: 4px;
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
  height: 55px;
  width: auto;
}
</style>
