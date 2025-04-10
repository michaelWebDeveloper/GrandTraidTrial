<template>
  <div class="name-search">
    <h1>Поиск имен</h1>
    <input
        type="text"
        @input="onInput"
        @keydown.down.prevent="onKeyDown('down')"
        @keydown.up.prevent="onKeyDown('up')"
        @keydown.tab.prevent="onKeyDown('up')"
        @keydown.enter.prevent="selectSuggestion"
        placeholder="Введите имя..."
    />
    <ul v-if="filteredNames.length > 0 && !hasSelectedValue" class="suggestions">
      <li
          v-for="(name, index) in filteredNames"
          :key="name"
          :class="{ active: index === activeIndex }"
          @click="selectName(name)"
          @mouseover="activeIndex = index"
      >
        {{ name }}
      </li>
    </ul>
    <div v-if="filteredNames.length === 0" class="no-results">
      Ничего не найдено
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import debounce from "lodash/debounce";


const names = [
  "Alice", "Bob", "Alex", "Алина", "Greg", "Светлана", "Дмитрий",
  "Ольга", "Hulio", "Иван", "Наталья", "Jack", "Татьяна", "Анна",
  "Александр", "Мария", "Павел", "Альхчибардушка", "Евгений", "Анастасия"
];

const query = ref('');
const activeIndex = ref(-1);
const hasSelectedValue = ref(false);

const filteredNames = computed(() => {
  if (!query.value) return [];
  return names.filter(name =>
      name.toLowerCase().includes(query.value.toLowerCase())
  );
});

const onInput = debounce((e) => {
  query.value = e.target.value;
  hasSelectedValue.value = false;
  activeIndex.value = -1; // Сброс активного индекса при новом вводе
}, 500);

const selectName = (name) => {
  hasSelectedValue.value = true;
  query.value = name;
  activeIndex.value = -1; // Сброс активного индекса
};

const selectSuggestion = () => {
  hasSelectedValue.value = true;
  if (activeIndex.value >= 0 && activeIndex.value < filteredNames.value.length) {
    query.value = filteredNames.value[activeIndex.value];
    activeIndex.value = -1; // Сброс активного индекса
  }
};

const onKeyDown = (direction) => {
  if (filteredNames.value.length === 0 || hasSelectedValue.value) return;

  if (direction === 'down') {
    activeIndex.value = (activeIndex.value + 1) % filteredNames.value.length;
  } else if (direction === 'up') {
    activeIndex.value = (activeIndex.value - 1 + filteredNames.value.length) % filteredNames.value.length;
  }
}


</script>

<style scoped>
.name-search {
  position: relative;
  width: 300px;
  background: white;
  border-radius: 4px;
  padding: 10px;
}
input {
  width: 100%;
  padding: 8px;
}
.suggestions {
  list-style: none;
  padding: 0;
  margin: 0;
  border: 1px solid #ccc;
  max-height: 200px;
  overflow-y: auto;
}
.suggestions li {
  padding: 8px;
  cursor: pointer;
}
.suggestions li.active {
  background-color: #007bff;
  color: white;
}
.no-results {
  margin-top: 8px;
}
</style>