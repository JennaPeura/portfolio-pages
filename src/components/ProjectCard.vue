<template>
  <q-card class="glass-card column full-height no-shadow overflow-hidden">
    <q-img v-if="project.image" :src="project.image" :ratio="16 / 9" class="project-image">
      <div class="absolute-full flex flex-center overlay-content">
        <q-btn
          round
          color="white"
          text-color="dark"
          icon="arrow_forward"
          @click="redirectToProjectSite(project.link)"
        />
      </div>
      <div class="absolute-top-right q-pa-sm">
        <q-badge
          :color="getStatusColor(project.status)"
          :label="project.status"
          class="q-py-xs q-px-sm text-weight-bold shadow-2"
        />
      </div>
    </q-img>
    <q-img
      v-else
      src="https://cdn.quasar.dev/img/parallax2.jpg"
      :ratio="16 / 9"
      class="project-image"
    >
      <div class="absolute-full flex flex-center overlay-content">
        <q-btn
          round
          color="white"
          text-color="dark"
          icon="arrow_forward"
          @click="redirectToProjectSite(project.link)"
        />
      </div>
      <div class="absolute-top-right q-pa-sm">
        <q-badge
          :color="getStatusColor(project.status)"
          :label="project.status"
          class="q-py-xs q-px-sm text-weight-bold shadow-2"
        />
      </div>
    </q-img>

    <q-card-section class="col q-pt-lg">
      <div class="text-overline text-secondary">{{ project.timeline }}</div>
      <div class="text-h5 text-weight-bold q-mb-sm text-white">{{ project.name }}</div>
      <div class="text-body2 text-grey-4" style="line-height: 1.6">
        {{ project.description }}
      </div>
    </q-card-section>

    <q-card-section class="q-pt-none">
      <div class="row q-gutter-sm">
        <q-chip
          v-for="tech in project.techStack"
          :key="tech"
          dense
          color="dark"
          text-color="primary"
          class="glass-chip"
        >
          {{ tech }}
        </q-chip>
      </div>
    </q-card-section>
  </q-card>
</template>

<script setup lang="ts">
import { useRouter } from 'vue-router';
import { type Project } from './models';

const router = useRouter();

interface Props {
  project: Project;
}

defineProps<Props>();

const getStatusColor = (status: string) => {
  switch (status) {
    case 'Launched':
      return 'green-6';
    case 'Open':
      return 'info';
    case 'Beta':
      return 'warning';
      case 'In Development':
      return 'orange';
    case 'Prototype':
      return 'grey-8';
    case 'Completed':
      return 'teal';
    default:
      return 'grey';
  }
};

const redirectToProjectSite = async (link: string | undefined) => {
  if (link) {
    if (link.startsWith('http')) {
      window.open(link, '_blank');
    } else {
      await router.push(link);
    }
  }
};
</script>

<style lang="scss" scoped>
.project-image {
  transition: transform 0.5s ease;

  .overlay-content {
    background: rgba(0, 0, 0, 0.4);
    opacity: 0;
    transition: opacity 0.3s ease;
    backdrop-filter: blur(2px);
  }
}

.glass-card:hover {
  .project-image {
    transform: scale(1.05);

    .overlay-content {
      opacity: 1;
    }
  }
}

.glass-chip {
  background: rgba(255, 255, 255, 0.05) !important;
  border: 1px solid rgba(255, 255, 255, 0.1);
}
</style>
