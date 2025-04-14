<template>
  <div class="second-tab">
    <h1>Second Tab</h1>
    <form class="second-tab__form">
      <input class="second-tab__input" placeholder="Some input"/>
      <button class="second-tab__button" type="button">Submit</button>
    </form>
  </div>
</template>
<script setup lang="ts">
</script>
<style scoped>
.second-tab__form {
  display: flex;
  gap: 16px;
}
.second-tab__input {
  padding: 4px;
  outline: none;
  border: 1px solid #fff;
  border-radius: 4px;
  background: transparent;
  color: white;
}

.second-tab__button {
  padding: 4px;
  background: transparent;
  color: white;
  border: 1px solid #fff;
  border-radius: 4px;
  outline: none;
  cursor: pointer;
  &:active {
    scale: 0.98;
  }
}
</style>
