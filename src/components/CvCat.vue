<script setup>
defineProps({
  icon: String,
  title: String,
})
</script>

<template>
  <div class="cv-cat">
    <div class="cv-cat-title pb-inside-avoid">
      <div class="cv-cat-icon">
        <i :class="icon"></i>
      </div>
      <h5 class="ps-3 m-0 fw-bold">{{ title }}</h5>
    </div>
    <div class="d-flex flex-column gap-3 mt-3">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.cv-cat-title {
  display: flex;
  align-items: center;
}

.cv-cat-icon {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: #263238;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.5rem;
}
</style>
