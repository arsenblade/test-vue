<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>
<script>
export default {
  name: 'my-button'
}
</script>
<style>
.btn {
  padding: 10px 15px;
  border: 1px solid green;
  background-color: transparent;
  cursor: pointer;
  border-radius: 4px;
}

.btn:hover {
  background-color: #00FF99;
}
</style>