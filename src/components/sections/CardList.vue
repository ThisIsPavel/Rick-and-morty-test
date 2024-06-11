<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import CardItem from "../CardItem.vue";
import ErrorSection from "./ErrorSection.vue";
import BaseInput from "../base/BaseInput.vue";
import BaseButton from "../base/BaseButton.vue";

const OPTION_LIST = ref([
  { name: "not selected", value: "" },
  { name: "Alive", value: "Alive" },
  { name: "Unknown", value: "unknown" },
  { name: "Dead", value: "Dead" },
]);

const serachCharacter = ref("");
const selectedStatus = ref("");
const pageNum = ref(1);
const urlPage = ref("https://rickandmortyapi.com/api/character/?page=1");
const prevPage = ref("");
const nextPage = ref("");
const cards = ref([]);
const error = ref(false);

const getCharacters = async () => {
  try {
    const { data } = await axios.get(
      `${urlPage.value}&name=${serachCharacter.value}&status=${selectedStatus.value}`
    );
    prevPage.value = data.info.prev;
    nextPage.value = data.info.next;
    console.log(data);
    cards.value = data.results;
    error.value = false;
  } catch (err) {
    error.value = true;
    console.log(err);
  }
};

onMounted(getCharacters);

const applyFilters = () => {
  getCharacters();
  pageNum.value = 1;
};

const goToPrevPage = () => {
  if (!prevPage.value) {
    return;
  }
  urlPage.value = prevPage.value;
  getCharacters();
  pageNum.value--;
};

const goToNextPage = () => {
  if (!nextPage.value) {
    return;
  }
  urlPage.value = nextPage.value;
  getCharacters();
  pageNum.value++;
};
</script>

<template>
  <section class="cardlist">
    <form class="filter" @submit.prevent="applyFilters">
      <base-input
        label="Имя"
        placeholder="Введите имя персонажа"
        type="text"
        :id="1"
        v-model="serachCharacter"
      />
      <select v-model="selectedStatus" class="select">
        <option
          v-for="OPTION in OPTION_LIST"
          :key="OPTION.name"
          :value="OPTION.value"
        >
          {{ OPTION.name }}
        </option>
      </select>
      <base-button text="Применить" type="submit" />
    </form>
    <div v-show="!error">
      <ul class="cards">
        <card-item
          v-for="card in cards"
          :key="card.id"
          :name="card.name"
          :img="card.image"
          :status="card.status"
          :species="card.species"
          :cardId="card.id"
          :location="card.location.name"
          :firstSeenUrl="card.episode[0]"
        />
      </ul>
      <div class="pagination">
        <ul class="pagination__list">
          <li class="pagination__prev">
            <base-button text="<" @click="goToPrevPage" />
          </li>
          <li class="pagination__num">{{ pageNum }}</li>
          <li class="pagination__nex">
            <base-button text=">" @click="goToNextPage" />
          </li>
        </ul>
      </div>
    </div>
    <error-section v-show="error" />
  </section>
</template>

<style>
:root {
  --marker-size: 0.9rem;
  --card-title-color: rgb(158, 158, 158);
  --card-border-radius: 0.5rem;
}
.cardlist {
  min-height: inherit;
  background: #272b33;
  padding: 8.1rem 0px;
}
.cards {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1rem;
}
.filter {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 5.5rem;
  margin-bottom: 3rem;
}
.pagination {
  margin-top: 3rem;
}
.pagination__list {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 2rem;
}
.pagination__num {
  color: white;
  font-size: 1.8rem;
}
.select {
  cursor: pointer;
  padding: 0.5rem;
}

@media (max-width: 548px) {
  .filter {
    flex-direction: column;
    margin: 0 auto 3rem;
    max-width: 500px;
  }
  .cardlist {
    padding: 8.1rem 2rem;
  }
  .select {
    width: 100%;
  }
}
</style>
