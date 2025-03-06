<template>
    <form class="flex flex-col border-2  p-10 justify-center items-center" action="#" @submit="handleCalculate">
        <label for="a">Enter a: </label>
        <input id="a" class="border-2 p-2" v-model.number="a" type="number" placeholder="Number A" />
        <label for="b">Enter a: </label>
        <input id="b" class="border-2 p-2" v-model.number="b" type="number" placeholder="Number B" />
        <label for="operator">Choose an operator: </label>
        <select id="operator" class="border-2" v-model="operator">
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
            <option value="%">%</option>
        </select>
        <button type="submit">Calculate</button>
        <p>{{ result }}</p>
    </form>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import { icp_workshops_backend } from 'declarations/icp_workshops_backend/index';

let a = ref(null);
let b = ref(null);
let operator = ref("+");
let result = ref("");

async function handleCalculate(e) {
    e.preventDefault();
    if (a.value === null || b.value === null) {
        result.value = "Please enter both numbers";
        return;
    }

    try {
        const response = await icp_workshops_backend.calculate(a.value, b.value, operator.value);
        result.value = response;
        localStorage.setItem('result', response);
    } catch (error) {
        result.value = error;
    }
}

onMounted(() => {
    const storedResult = localStorage.getItem('result');
    if (storedResult) {
        result.value = storedResult;
    }
})
</script>

<style lang="scss" scoped></style>