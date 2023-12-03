<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount
        <br />
        (negative - expense, positive - income)
      </label>
      <input
        type="text"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">{{ item.id ? "Update" : "Add" }} transaction</button>
  </form>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import { useToast } from "vue-toast-notification";

const props = defineProps({
  item: {
    type: Object,
    default() {
      return { id: "", text: "", amount: "" };
    },
  },
});

const emit = defineEmits(["submit"]);

// TODO: update item
const text = ref(props.item.text);
const amount = ref(props.item.amount);

const toast = useToast();

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error("Please input text and amount");
  }

  emit("submit", {
    text: text.value,
    amount: parseFloat(amount.value),
  });

  text.value = "";
  amount.value = "";
};
</script>