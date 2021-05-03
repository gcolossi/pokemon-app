<template>
    <div class="loader"></div>
</template>

<script>
export default {
    
}
</script>

<style scoped>

.loader {
  border: 16px solid rgb(44, 44, 44);
  border-top: 16px solid rgb(116, 116, 116);
  border-radius: 50%;
  width: 80px;
  height: 80px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

</style>