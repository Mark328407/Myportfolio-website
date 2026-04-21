<template>
  <section id="tools" class="tools-section">
    <div class="container">
      <h2 class="tools-title">Tools</h2>
      <div class="row">
        <div
          v-for="group in toolGroups"
          :key="group.label"
          class="col-12 col-md-6 col-lg-4"
        >
          <div class="tools-card">
            <h4>{{ group.label }}</h4>
            <div class="icons">
              <i
                v-for="icon in group.icons"
                :key="icon"
                :class="icon"
              ></i>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const toolGroups = ref([
  {
    label: 'Front End',
    icons: [
      'devicon-html5-plain colored',
      'devicon-css3-plain colored',
      'devicon-bootstrap-plain colored',
      'devicon-javascript-plain colored'
    ]
  },
  {
    label: 'MERN',
    icons: [
      'devicon-mongodb-plain colored',
      'fa-solid fa-server',
      'devicon-react-original colored',
      'devicon-nodejs-plain colored'
    ]
  },
  {
    label: 'Other Tools',
    icons: [
      'devicon-git-plain colored',
      'devicon-github-original',
      'fa-solid fa-paper-plane text-warning',
      'fa-solid fa-code text-secondary'
    ]
  }
])
</script>