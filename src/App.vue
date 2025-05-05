<script setup>
import {onMounted, ref} from "vue";
import CvHeader from "@/components/CvHeader.vue";
import CvCat from "@/components/CvCat.vue";

const cv = ref({})

onMounted(async () => {
  const urlSearchParams = new URLSearchParams(window.location.search)
  const params = Object.fromEntries(urlSearchParams.entries())
  const id = params.id ?? 1
  const res = await fetch(`https://phuongbv00.github.io/cv-data/${id}.json`)
  cv.value = await res.json()
})

function fmtDate(year, month, fmtForNullYear = 'present') {
  const months = ['jan', 'feb', 'mar', 'apr', 'may', 'jun', 'jul', 'aug', 'sep', 'oct', 'nov', 'dec']
  if (!year)
    return fmtForNullYear
  else if (!month)
    return year
  return `${months[month - 1]} ${year}`
}

function fmtDuration(startYear, startMonth, endYear, endMonth) {
  const now = new Date()
  endYear = endYear ?? now.getFullYear()
  endMonth = endMonth ?? (now.getMonth() + 1)

  let totalMonths = (endYear - startYear) * 12 + (endMonth - startMonth) + 1
  const years = Math.floor(totalMonths / 12)
  const months = totalMonths % 12

  const parts = []
  if (years > 0) parts.push(`${years} ${years > 1 ? 'yrs' : 'yr'}`)
  if (months > 0) parts.push(`${months} ${months > 1 ? 'mos' : 'mo'}`)

  return parts.join(' ') || 'Less than 1 mo'
}
</script>

<template>
  <div class="cv d-flex flex-column">
    <CvHeader :cv="cv"/>
    <div class="cv-body p-5 flex-grow-1 d-grid gap-5">
      <p v-if="cv.about" class="m-0 text-justify">{{ cv.about }}</p>
      <CvCat icon="bi bi-book-fill" title="EDUCATION" v-if="cv.edu">
        <div v-for="edu in cv.edu" class="pb-inside-avoid">
          <div class="d-flex">
            <div><b class="text-uppercase" v-html="edu.school"></b></div>
            <div class="ms-auto">
              <small>
                <b class="text-uppercase">
                  {{ fmtDate(edu.startDtYr, edu.startDtMo) }}
                  -
                  {{ fmtDate(edu.endDtYr, edu.endDtMo) }}
                </b>
              </small>
            </div>
          </div>
          <div class="fw-light">
            <small>{{ edu.degree }}, {{ edu.spec }}</small>
            <small class="d-block text-secondary" v-if="edu.grade">Grade: {{ edu.grade }}</small>
          </div>
        </div>
      </CvCat>
      <CvCat icon="bi bi-wallet-fill" title="EXPERIENCE" v-if="cv.exp">
        <div v-for="exp in cv.exp" class="pb-inside-avoid">
          <div class="d-flex">
            <div><b class="text-uppercase">{{ exp.title }}</b></div>
            <div class="ms-auto"><small><b class="text-uppercase">{{
                fmtDate(exp.startDtYr, exp.startDtMo)
              }} -
              {{ fmtDate(exp.endDtYr, exp.endDtMo) }}</b></small></div>
          </div>
          <div class="fw-light"><small>{{ exp.company }} • {{ exp.empType }} • {{
              fmtDuration(exp.startDtYr,
                  exp.startDtMo,
                  exp.endDtYr, exp.endDtMo)
            }}</small></div>
          <div class="text-secondary fw-light"><small>{{ exp.location }}</small></div>
        </div>
      </CvCat>
      <CvCat icon="bi bi-patch-check-fill" title="CERTIFICATES" v-if="cv.certs">
        <div v-for="cert in cv.certs" class="pb-inside-avoid">
          <div class="d-flex fw-bold text-uppercase">
            <div>{{ cert.title }}</div>
            <div class="ms-auto">
              <small>{{ fmtDate(cert.issuedDtYr, cert.issuedDtMo) }}</small>
            </div>
          </div>
          <div class="fw-light"><small>{{ cert.author }}</small></div>
          <small class="d-block fw-light text-secondary">
            <a :href="cert.link" class="text-secondary" target="_blank">
              {{ cert.link }}
              <i style="font-size: 10px;" class="bi bi-box-arrow-up-right"></i>
            </a>
          </small>
        </div>
      </CvCat>
      <CvCat icon="bi bi-pen-fill" title="SKILLS" v-if="cv.skills">
        <ul style="list-style: square" class="mb-0 ms-1">
          <li v-for="(skv, skk) in cv.skills" class="fw-light pb-inside-avoid">{{ skk }}: <b>{{ skv }}</b></li>
        </ul>
      </CvCat>
      <CvCat icon="bi bi-code" title="PROJECTS" v-if="cv.projects">
        <div v-for="prj in cv.projects" class="pb-inside-avoid">
          <div class="d-flex">
            <div><b class="text-uppercase">{{ prj.name }}</b></div>
            <div class="ms-auto"><small><b class="text-uppercase">{{
                fmtDate(prj.startDtYr, prj.startDtMo)
              }} -
              {{ fmtDate(prj.endDtYr, prj.endDtMo) }}</b></small></div>
          </div>
          <small v-if="prj.company" class="d-block fw-light text-secondary">{{ prj.company }}</small>
          <div class="mt-2 ms-4 ps-2 border-start border-3 fw-light">
            <p><small>{{ prj.des }}</small></p>
            <p>
              <small class="d-block">Market: <b>{{ prj.market }}</b></small>
              <small class="d-block">Technologies: <b>{{ prj.tech.join(', ') }}</b></small>
              <small class="d-block" v-if="prj.team">Team: <b>{{ prj.team }}</b></small>
            </p>
            <div>
              <small>Role(s): <b>{{ prj.role }}</b></small>
              <small>
                <ul style="list-style: square">
                  <li v-for="prjRole in prj.roleDes" class="fw-light">{{ prjRole }}</li>
                </ul>
              </small>
            </div>
          </div>
        </div>
      </CvCat>
    </div>
  </div>
</template>

<style scoped>
.cv {
  width: 820px !important;
}

.cv-body {
  background-color: white;
}
</style>
