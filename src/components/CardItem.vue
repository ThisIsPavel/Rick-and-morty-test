<script setup>
import axios from "axios";
import { computed, defineProps, onMounted, ref } from "vue";

const props = defineProps({
  name: String,
  img: String,
  status: String,
  species: String,
  cardId: Number,
  location: String,
  firstSeenUrl: String,
});

const firstSeen = ref("");

const getSceen = async () => {
  try {
    const { data } = await axios.get(props.firstSeenUrl);
    firstSeen.value = data.name;
  } catch (err) {
    console.log(err);
  }
};

onMounted(getSceen);

const markerColor = computed(() => {
  const status = props.status.toLowerCase();
  switch (status) {
    case "alive":
      return "green";
    case "dead":
      return "red";
    default:
      return "grey";
  }
});
</script>

<template>
  <li class="cards__item">
    <div class="cards__left">
      <img class="cards__img" :src="img" :alt="name" />
    </div>
    <ul class="cards__right">
      <li class="cards__header">
        <h2 class="cards__title">{{ name }}</h2>
        <p class="cards__status">
          <span :class="`cards__marker ${markerColor}`"></span>{{ status }} -
          {{ species }}
        </p>
      </li>
      <li class="cards__middle">
        <p class="cards__location-subtitle subtitle-card">
          Last known location:
        </p>
        <p class="cards__location-name">{{ location }}</p>
      </li>
      <li class="cards__footer">
        <p class="cards__episode-subtitle subtitle-card">First seen in:</p>
        <p class="cards__episode-name">{{ firstSeen }}</p>
      </li>
    </ul>
  </li>
</template>

<style>
.cards__item {
  width: 60rem;
  min-height: 22rem;
  display: flex;
  background: rgb(60, 62, 68);
  border-radius: var(--card-border-radius);
  margin: 0.75rem;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px,
    rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}
.cards__img {
  height: 100%;
  width: 100%;
  object-position: center center;
  object-fit: cover;
  border-radius: var(--card-border-radius) 0 0 var(--card-border-radius);
}
.cards__left {
  flex: 2;
}
.cards__right {
  font-size: 1.6rem;
  font-weight: 500;
  flex: 3;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-start;
  padding: 1.3rem;
  color: rgb(255, 255, 255);
}
.cards__title {
  font-size: 2.7rem;
}
.cards__status {
  padding-left: calc(var(--marker-size) * 2);
  position: relative;
}
.cards__marker {
  position: absolute;
  width: var(--marker-size);
  height: var(--marker-size);
  left: 0;
  top: calc(50% - (var(--marker-size) / 2));
  border-radius: 50%;
}
.cards__marker.green {
  background: green;
}
.cards__marker.red {
  background: red;
}
.cards__marker.grey {
  background: grey;
}
/* .cards__status::before {
  position: absolute;
  content: "";
  width: var(--marker-size);
  height: var(--marker-size);
  left: 0;
  top: calc(50% - (var(--marker-size) / 2));
  border-radius: 50%;
  background: red;
} */
.subtitle-card {
  color: var(--card-title-color);
}
.cards__location-name {
  font-size: 1.8rem;
}
.cards__episode-name {
  font-size: 1.8rem;
}

@media (max-width: 548px) {
  .cards__item {
    flex-direction: column;
  }
  .cards__right {
    gap: 3rem;
  }
}
</style>
