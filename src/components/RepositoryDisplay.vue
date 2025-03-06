<template>
  <div>
    <div v-if="loading" class="loading">
      Loading, please wait.
    </div>

    <div v-if="error" class="error">
      Error fetching repositories. <ion-button @click="onRetry">Click to retry</ion-button>
    </div>

    <div v-if="repo" class="repo-info">
      <h2>{{ repo.name }}</h2>
      <p>{{ repo.description }}</p>
      <p>
        <ion-icon  :md="ellipse" color="warning"></ion-icon>
        {{ repo.language }} |
      <p> <ion-icon :md="star" color="danger"></ion-icon>
        {{ repo.stargazers_count }} | <ion-icon :md="gitBranchOutline"></ion-icon>
        {{ repo.forks_count }} | <ion-icon :md="alertCircleOutline" color="danger"></ion-icon> {{ repo.open_issues_count }}</p>

      </p>
      <ion-button @click="onRefresh">Refresh</ion-button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
import { IonButton, IonIcon } from '@ionic/vue';
import { gitBranchOutline, alertCircleOutline, star, ellipse } from 'ionicons/icons';

// Definir las props que recibe el componente
const props = defineProps({
  repo: {
    type: Object,
    default: null,
  },
  loading: {
    type: Boolean,
    default: false,
  },
  error: {
    type: Boolean,
    default: false,
  },
});

// Emitir eventos para refrescar o reintentar
const emit = defineEmits(['refresh', 'retry']);

const onRefresh = () => {
  emit('refresh'); // Emitir el evento 'refresh'
};

const onRetry = () => {
  emit('retry'); // Emitir el evento 'retry'
};
</script>

<style scoped>
.loading,
.error {
  text-align: center;
  margin-top: 20px;
}

.repo-info {
  text-align: center;
  margin-top: 20px;
}

.repo-info p {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  /* Espacio entre íconos y texto */
}
</style>