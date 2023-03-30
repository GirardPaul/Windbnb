<script setup>
defineProps({
  stay: {
    type: Object,
  },
});
</script>

<template>
  <div class="flex column">
    <img :src="stay.photo" :alt="stay.title" />
    <div class="flex align-center justify-between mt-20 h-30">
      <div class="flex align-center">
        <p v-show="stay.superHost" class="superhost">SUPER HOST</p>
        <p :class="['description', stay.superHost ? 'ml-11' : '']">
          {{ stay.type }} <span v-if="stay.beds">. {{ stay.beds }} beds</span>
        </p>
      </div>
      <div class="flex align-center">
        <span class="stars material-icons">star</span>
        <p class="rating">{{ stay.rating }}</p>
      </div>
    </div>
    <p class="title mt-17">{{ stay.title }}</p>
  </div>
</template>

<style scoped>
.h-30 {
  height: 30px;
}
.mt-17 {
  margin-top: 17px;
}
p.title {
  color: #333333;
  font-weight: 600;
  font-size: 16px;
}
.description {
  font-weight: 500;
  font-size: 14px;
  color: #828282;
}
p.rating {
  font-weight: 500;
  font-size: 14px;
  color: #4f4f4f;
  margin-left: 7px;
}
.stars {
  color: #eb5757;
}
.mt-20 {
  margin-top: 20px;
}
.ml-11 {
  margin-left: 11px;
}
p.superhost {
  color: #4f4f4f;
  border: 1px solid #4f4f4f;
  border-radius: 12px;
  padding: 7px 9px;
  font-size: 12px;
  font-weight: 700;
}
p {
  margin: 0;
}
img {
  height: 269px;
  width: 100%;
  border-radius: 24px;
}

@media screen and (max-width: 576px) {
  img {
    height: 239px;
  }
}
</style>
