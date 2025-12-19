<template>
  <q-page class="q-pa-md">
    <div class="row justify-center q-mb-xl">
      <div class="col-12 col-md-8 text-center">
        <h2 class="text-h3 text-weight-bold text-gradient q-mb-md">About Me</h2>
        <p class="text-h6 text-grey-4 text-justify" style="line-height: 1.8">
          As Co-founder & CTO of <b>Escapica</b>, I lead the technical vision and execution of a
          full-stack web platform built from scratch. I manage the entire development process, from
          <b>architecture</b> and <b>frontend implementation</b> to <b>infrastructure</b>,
          <b>deployment</b>, and <b>team leadership</b>.
        </p>

        <p class="text-h6 text-grey-4 text-justify" style="line-height: 1.8">
          I tackle complex technical and operational challenges, ensuring the platform is
          <b>scalable</b>, <b>secure</b>, and <b>maintainable</b>. My expertise includes
          <b>Vue 3</b>, <b>Quasar</b>, <b>TypeScript</b>, and backend infrastructure with
          <b>self-hosted Supabase</b>, <b>Docker</b>, and <b>Linux</b>.
        </p>
      </div>
    </div>

    <div class="row justify-center q-mb-xl">
      <div class="col-12 col-md-8">
        <h3 class="text-h4 text-weight-bold text-secondary q-mb-lg text-center">Skills</h3>

        <div v-for="(categorySkills, category) in skills" :key="category" class="q-mb-lg">
          <div class="text-h6 text-grey-4 q-mb-sm text-center">{{ category }}</div>
          <div class="row justify-center q-gutter-md">
            <q-chip
              v-for="skill in categorySkills"
              :key="skill"
              color="dark"
              text-color="primary"
              icon="check"
              class="glass-chip q-px-md q-py-sm"
            >
              {{ skill }}
              <q-tooltip class="text-subtitle bg-teal-8" anchor="bottom middle" self="top middle">
                Proficient with {{ skill }}
              </q-tooltip>
            </q-chip>
          </div>
        </div>
      </div>
    </div>

    <div class="row justify-center">
      <div class="col-12 col-md-8">
        <TimelineSection :events="timelineEvents" />
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import TimelineSection from 'components/TimelineSection.vue';
import { type TimelineEvent } from 'components/models';

const skills = {
  Frontend: ['Vue 3', 'Quasar Framework', 'TypeScript', 'SCSS', 'HTML5', 'CSS3'],
  Backend: ['Node.js', 'Supabase', 'PostgreSQL'],
  DevOps: ['Docker', 'Linux', 'Git', 'GitHub Actions'],
  Tools: ['VS Code', 'Figma', 'Postman'],
};

const timelineEvents = ref<TimelineEvent[]>([
  {
    id: 1,
    title: 'Project Inception & Architecture',
    date: '06/2024',
    description:
      'Co-founded Escapica, defined the technical vision, architecture, and stack, and started full-stack development.',
    color: 'purple',
    icon: 'architecture',
  },
  {
    id: 2,
    title: 'Closed Beta Release',
    date: '06/2025',
    description:
      'Released beta to a controlled user group, validating core functionality, security, and infrastructure. Collected feedback for improvements.',
    color: 'orange',
    icon: 'security',
  },
  {
    id: 3,
    title: 'Open Beta Expansion',
    date: '09/2025',
    description:
      'Expanded access to the public, optimized performance, and scaled API integrations.',
    color: 'teal',
    icon: 'public',
  },
  {
    id: 4,
    title: 'Planned Official Platform Launch',
    date: '03/2026',
    description: 'Full platform release, marking the culmination of development from scratch.',
    color: 'green',
    icon: 'rocket_launch',
  },
]);
</script>
