<script setup>
import NavBar from './components/NavBar.vue'
import HeroSection from './components/HeroSection.vue'
import ProjectsSection from './components/ProjectsSection.vue'
import ToolsSection from './components/ToolsSection.vue'
import ContactSection from './components/ContactSection.vue'
import FooterSection from './components/FooterSection.vue'
</script>

<template>
  <NavBar />
  <div class="container-fluid">
    <HeroSection />
    <ProjectsSection />
    <ToolsSection />
    <ContactSection />
  </div>
  <FooterSection />
</template>

<style scoped></style>