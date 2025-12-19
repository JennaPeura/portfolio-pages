<template>
  <q-layout view="hHh lpR fFf" class="bg-transparent text-white">
    <!-- Background Mesh -->
    <div class="bg-mesh"></div>

    <q-header class="glass text-white q-py-sm" style="background: rgba(2, 6, 23, 0.8)">
      <q-toolbar>
        <q-toolbar-title>
          <q-btn flat no-caps to="/" class="text-h5 text-weight-bold q-px-none hover-effect">
            <span class="text-gradient">Jenna</span>
          </q-btn>
        </q-toolbar-title>

        <div class="gt-xs row q-gutter-sm">
          <q-btn flat rounded to="/projects" label="Projects" class="nav-btn" />
          <q-btn flat rounded to="/about" label="About" class="nav-btn" />

          <div class="q-mx-md separator-vertical"></div>

          <q-btn
            flat
            round
            dense
            icon="code"
            type="a"
            href="https://github.com/JennaPeura"
            target="_blank"
            class="social-btn"
          />
          <q-btn
            flat
            round
            dense
            icon="work"
            type="a"
            href="https://linkedin.com/in/jenna-peura-0ba446185/"
            target="_blank"
            class="social-btn"
          />
          <q-btn
            flat
            round
            dense
            icon="email"
            type="a"
            href="mailto:jenna.peura@hotmail.com"
            class="social-btn"
          />
        </div>

        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          class="lt-sm"
          @click="toggleRightDrawer"
        />
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="rightDrawerOpen"
      side="right"
      overlay
      behavior="mobile"
      class="glass-drawer bg-dark"
    >
      <div class="column full-height q-pa-md">
        <div class="text-h4 q-mb-xl text-weight-bold text-gradient">Menu</div>

        <q-list padding class="text-h6">
          <q-item clickable to="/" exact v-ripple class="rounded-borders q-mb-sm">
            <q-item-section>Home</q-item-section>
          </q-item>

          <q-item clickable to="/projects" v-ripple class="rounded-borders q-mb-sm">
            <q-item-section>Projects</q-item-section>
          </q-item>

          <q-item clickable to="/about" v-ripple class="rounded-borders q-mb-sm">
            <q-item-section>About</q-item-section>
          </q-item>
        </q-list>

        <q-space />

        <div class="row justify-center q-gutter-md q-mb-lg">
          <q-btn
            flat
            round
            icon="code"
            type="a"
            href="https://github.com/JennaPeura"
            target="_blank"
          />
          <q-btn
            flat
            round
            icon="work"
            type="a"
            href="https://www.linkedin.com/in/jenna-peura-0ba446185/"
            target="_blank"
          />
          <q-btn flat round icon="email" type="a" href="mailto:jenna.peura@hotmail.com" />
        </div>
      </div>
    </q-drawer>

    <q-page-container>
      <router-view v-slot="{ Component }">
        <transition name="fade" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </q-page-container>

    <q-footer class="bg-transparent text-grey-5 q-py-lg">
      <div class="text-center text-caption">
        &copy; {{ new Date().getFullYear() }} Jenna. Built with
        <span class="text-primary">Quasar</span> & <span class="text-secondary">Vue 3</span>.
      </div>
    </q-footer>

    <q-page-scroller position="bottom-right" :scroll-offset="150" :offset="[18, 18]">
      <q-btn fab icon="keyboard_arrow_up" color="primary" text-color="dark" />
    </q-page-scroller>

    <q-dialog v-model="commandPaletteOpen" position="top">
      <q-card style="width: 600px; max-width: 90vw" class="glass-card q-mt-xl">
        <q-card-section class="q-pa-none">
          <q-input
            dark
            v-model="searchQuery"
            placeholder="Type a command or search..."
            borderless
            class="q-px-md q-py-sm text-h6"
            autofocus
            color="primary"
            input-class="text-white"
          >
            <template v-slot:prepend>
              <q-icon name="search" />
            </template>
          </q-input>
        </q-card-section>

        <q-separator color="grey-8" />

        <q-card-section class="q-pa-none" style="max-height: 300px; overflow-y: auto">
          <q-list separator>
            <q-item
              v-for="cmd in filteredCommands"
              :key="cmd.id"
              clickable
              v-ripple
              @click="executeCommand(cmd)"
              class="q-py-md"
            >
              <q-item-section avatar>
                <q-icon :name="cmd.icon" color="primary" />
              </q-item-section>
              <q-item-section class="text-body1">{{ cmd.label }}</q-item-section>
              <q-item-section side v-if="cmd.id === 'github' || cmd.id === 'linkedin'">
                <q-icon name="open_in_new" size="xs" />
              </q-item-section>
            </q-item>
            <q-item v-if="filteredCommands.length === 0" class="q-py-md">
              <q-item-section class="text-center text-grey">No results found</q-item-section>
            </q-item>
          </q-list>
        </q-card-section>

        <q-separator color="grey-8" />

        <q-card-section class="bg-dark-transparent q-py-xs">
          <div class="row justify-between text-caption text-grey">
            <div>
              <span class="q-mr-sm">Navigate</span>
              <q-icon name="keyboard_arrow_up" />
              <q-icon name="keyboard_arrow_down" />
            </div>
            <div>
              <span class="q-mr-sm">Select</span>
              <q-icon name="keyboard_return" />
            </div>
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </q-layout>
</template>

<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const rightDrawerOpen = ref(false);
const commandPaletteOpen = ref(false);
const searchQuery = ref('');

const commands = [
  { id: 'home', label: 'Home', icon: 'home', action: () => router.push('/') },
  { id: 'projects', label: 'Projects', icon: 'code', action: () => router.push('/projects') },
  { id: 'about', label: 'About', icon: 'person', action: () => router.push('/about') },
  {
    id: 'github',
    label: 'GitHub',
    icon: 'code',
    action: () => window.open('https://github.com/JennaPeura', '_blank'),
  },
  {
    id: 'linkedin',
    label: 'LinkedIn',
    icon: 'work',
    action: () => window.open('https://www.linkedin.com/in/jenna-peura-0ba446185/', '_blank'),
  },
  {
    id: 'email',
    label: 'Email',
    icon: 'email',
    action: () => (window.location.href = 'mailto:jenna.peura@hotmail.com'),
  },
];

const filteredCommands = computed(() => {
  if (!searchQuery.value) return commands;
  const query = searchQuery.value.toLowerCase();
  return commands.filter((cmd) => cmd.label.toLowerCase().includes(query));
});

const executeCommand = (command: (typeof commands)[0]) => {
  void command.action();
  commandPaletteOpen.value = false;
  searchQuery.value = '';
};

const toggleRightDrawer = () => {
  rightDrawerOpen.value = !rightDrawerOpen.value;
};

const handleKeydown = (e: KeyboardEvent) => {
  if ((e.ctrlKey || e.metaKey) && e.key === 'k') {
    e.preventDefault();
    commandPaletteOpen.value = true;
  }
};

onMounted(() => {
  window.addEventListener('keydown', handleKeydown);
});

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeydown);
});
</script>

<style lang="scss" scoped>
.nav-btn {
  font-weight: 500;
  letter-spacing: 0.5px;
  transition: color 0.3s;

  &:hover {
    color: $primary;
  }
}

.social-btn {
  opacity: 0.7;
  transition: all 0.3s;

  &:hover {
    opacity: 1;
    color: $secondary;
    transform: translateY(-2px);
  }
}

.separator-vertical {
  width: 1px;
  background: rgba(255, 255, 255, 0.1);
  height: 24px;
  align-self: center;
}

.glass-drawer {
  background: rgba(2, 6, 23, 0.95) !important;
  backdrop-filter: blur(10px);
}

.fade-enter-active,
.fade-leave-active {
  transition:
    opacity 0.4s ease,
    transform 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}

.bg-dark-transparent {
  background: rgba(0, 0, 0, 0.3);
}
</style>
