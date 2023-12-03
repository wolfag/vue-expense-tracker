<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpense :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @remove="handleRemoveTransaction"
      @select="handleSelectTransaction"
    />
    <AddTransaction @submit="handleAddTransaction" :item="selectedItem" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpense.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { ref, computed, watch, onMounted } from "vue";
import { uuid } from "vue-uuid";
import { useToast } from "vue-toast-notification";

const toast = useToast();
const transactions = ref([]);
const selectedItem = ref({ id: "", text: "", amount: "" });

onMounted(() => {
  const transactionList = JSON.parse(localStorage.getItem("transactionList"));

  if (transactionList) {
    transactions.value = transactionList;
  }
});

watch(
  transactions,
  () => {
    localStorage.setItem("transactionList", JSON.stringify(transactions.value));
  },
  { deep: true }
);

const total = computed(() => {
  return transactions.value.reduce((prev, cur) => prev + cur.amount, 0);
});

const income = computed(() => {
  return transactions.value
    .reduce((prev, cur) => (cur.amount > 0 ? prev + cur.amount : 0), 0)
    .toFixed(2);
});

const expenses = computed(() => ((total.value - income.value) * -1).toFixed(2));

const handleAddTransaction = (data) => {
  transactions.value.push({
    id: uuid.v1(),
    ...data,
  });

  toast.success("Added");
};

const handleRemoveTransaction = (id) => {
  transactions.value = transactions.value.filter((item) => item.id !== id);
  toast.success("Removed");

  if (id === selectedItem.id) {
    selectedItem.value = { id: "", text: "", amount: "" };
  }
};

const handleSelectTransaction = (id) => {
  const item = transactions.value.find((item) => item.id === id);
  if (item) {
    selectedItem.value = item;
  }
};
</script>
