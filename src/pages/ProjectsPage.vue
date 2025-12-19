<template>
  <q-page class="q-pa-md">
    <div class="text-center q-mb-xl q-pt-lg">
      <h2 class="text-h2 text-weight-bold text-gradient q-mb-sm">My Projects</h2>
      <p class="text-h6 text-grey-5">A selection of my recent work</p>
    </div>

    <div class="row justify-center q-mb-xl q-gutter-sm">
      <q-chip
        clickable
        :color="selectedTech === null ? 'primary' : 'dark'"
        :text-color="selectedTech === null ? 'dark' : 'white'"
        @click="selectedTech = null"
        label="All"
        :class="{ 'glass-chip': selectedTech !== null }"
      />
      <q-chip
        v-for="tech in allTech"
        :key="tech"
        clickable
        :color="selectedTech === tech ? 'primary' : 'dark'"
        :text-color="selectedTech === tech ? 'dark' : 'white'"
        @click="selectedTech = tech"
        :label="tech"
        :class="{ 'glass-chip': selectedTech !== tech }"
      />
    </div>

    <div class="row q-col-gutter-xl">
      <div v-for="project in filteredProjects" :key="project.id" class="col-12 col-md-6 col-lg-4">
        <ProjectCard :project="project" />
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import ProjectCard from 'components/ProjectCard.vue';
import { type Project } from 'components/models';

const selectedTech = ref<string | null>(null);

const projects = ref<Project[]>([
  {
    id: 1,
    name: 'Escapica',
    description:
      'Full-stack booking and discovery platform for immersive experiences. Built with Vue 3, Quasar, and Supabase, featuring real-time availability, secure payments, and a scalable architecture.',
    techStack: ['Vue 3', 'Quasar', 'PostgreSQL', 'TypeScript', 'SCSS'],
    status: 'In Development',
    timeline: 'Founded 06/2024',
    image: 'escapica-preview.png',
    link: 'https://escapica.com',
  },
  {
    id: 2,
    name: 'Personal Portfolio',
    description: 'My personal portfolio website showcasing my skills and projects.',
    techStack: ['Vue 3', 'Quasar', 'SCSS'],
    status: 'Launched',
    timeline: 'Launched 12/2025',
    image: 'portfolio.png',
    link: 'https://github.com/JennaPeura/portfolio-pages',
  },
  {
    id: 3,
    name: 'Starshifted',
    description: 'A social site prototype built with Vue and responsive design principles.',
    techStack: ['Vue 3', 'Quasar', 'CSS'],
    status: 'Prototype',
    timeline: '2022',
    image: 'starshifted.png',
    link: 'https://github.com/JennaPeura/starshifted',
  },
    {
    id: 4,
    name: 'Better YouTube',
    description: 'A better looking YouTube.',
    techStack: ['React', 'Material UI', 'CSS'],
    status: 'Completed',
    timeline: '2022',
    image: 'ytclone.png',
    link: 'https://github.com/JennaPeura/BetterYouTube',
  },
]);

const allTech = computed(() => {
  const techSet = new Set<string>();
  projects.value.forEach((p) => p.techStack.forEach((t) => techSet.add(t)));
  return Array.from(techSet).sort();
});

const filteredProjects = computed(() => {
  if (!selectedTech.value) return projects.value;
  return projects.value.filter((p) => p.techStack.includes(selectedTech.value!));
});
</script>
