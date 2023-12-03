<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="item in transactions"
      :key="item.id"
      :class="item.amount < 0 ? 'minus' : 'plus'"
      @click="selectItemId(item.id)"
    >
      {{ item.text }} <span>${{ item.amount }}</span>
      <button class="delete-btn" @click="removeTransaction(item.id)">x</button>
    </li>
  </ul>
</template>

<script setup>
const emit = defineEmits(["remove", "select"]);

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const removeTransaction = (id) => {
  emit("remove", id);
};

const selectItemId = (id) => {
  emit("select", id);
};
</script>