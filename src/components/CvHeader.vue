<script setup>
defineProps({
  cv: Object
})
</script>

<template>
  <div class="cv-header text-white d-flex">
    <div class="cv-header-1 d-flex flex-column justify-content-center p-4">
      <h4 class="fw-bold text-uppercase text-center">{{ cv.fullName }}</h4>
      <i class="text-center">{{ cv.career }}</i>
    </div>
    <div class="cv-header-2 p-4 d-flex flex-column gap-2">
      <div class="d-flex align-items-center" v-if="cv.birthDate">
        <i class="bi bi-calendar-week"></i>
        <span class="ps-2">{{ cv.birthDate }}</span>
      </div>
      <div class="d-flex align-items-center" v-if="cv.gender">
        <i class="bi bi-gender-ambiguous"></i>
        <span class="ps-2">{{ cv.gender }}</span>
      </div>
      <div class="d-flex align-items-center" v-if="cv.phone">
        <i class="bi bi-telephone"></i>
        <a class="ps-2 text-white" :href="'tel:' + cv.phone" target="_blank">
          {{ cv.phone }}
          <i style="font-size: 10px;" class="bi bi-box-arrow-up-right"></i>
        </a>
      </div>
      <div class="d-flex align-items-center" v-if="cv.mail">
        <i class="bi bi-envelope"></i>
        <a class="ps-2 text-white" :href="'mailto:' + cv.mail" target="_blank">
          {{ cv.mail }}
          <i style="font-size: 10px;" class="bi bi-box-arrow-up-right"></i>
        </a>
      </div>
      <div class="d-flex align-items-center" v-if="cv.address">
        <i class="bi bi-geo-alt"></i>
        <span class="ps-2">{{ cv.address }}</span>
      </div>
      <div class="d-flex align-items-center" v-if="cv.linkedin">
        <i class="bi bi-linkedin"></i>
        <a class="ps-2 text-white" :href="cv.linkedin" target="_blank">
          {{ cv.linkedin }}
          <i style="font-size: 10px;" class="bi bi-box-arrow-up-right"></i>
        </a>
      </div>
      <div class="d-flex align-items-center" v-if="cv.github">
        <i class="bi bi-github"></i>
        <a class="ps-2 text-white" :href="cv.github" target="_blank">
          {{ cv.github }}
          <i style="font-size: 10px;" class="bi bi-box-arrow-up-right"></i>
        </a>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cv-header {
  background-color: #37474f;
}

.cv-header-1 {
  background-color: #263238;
}

.cv-header-2 {
  font-size: 14px;
}
</style>
