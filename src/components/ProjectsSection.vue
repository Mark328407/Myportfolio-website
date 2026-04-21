<template>
  <section id="projects" class="projects-section py-5">
    <div class="container">
      <h2 class="text-center mb-5 projects-heading">My Projects</h2>
      <div class="row g-4 justify-content-center">
        <div
          v-for="project in projects"
          :key="project.id"
          class="col-lg-4 col-md-6"
        >
          <div class="card project-card h-100">
            <img :src="project.image" class="card-img-top p-4" :alt="project.title" />
            <div class="card-body">
              <h5 class="card-title">{{ project.title }}</h5>
              <p class="card-text">{{ project.description }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'
import projectsData from '../data/projects.json'

const projects = ref(projectsData)
</script>