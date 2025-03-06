<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>GitHub Repository Finder</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <!-- Selector de Lenguaje -->
      <LanguageSelector @language-change="onLanguageChange" />
      
      <!-- Visualización del Repositorio -->
      <RepositoryDisplay
        :repo="repo"
        :loading="loading"
        :error="error"
        @refresh="onRefresh"
        @retry="onRetry"
      />
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent} from '@ionic/vue';

// Componentes hijos
import LanguageSelector from '@/components/LanguageSelector.vue';
import RepositoryDisplay from '@/components/RepositoryDisplay.vue';

// Variables reactivas
const selectedLanguage = ref('');
const repo = ref(null);
const loading = ref(false);
const error = ref(false);

// Función para actualizar el lenguaje seleccionado
const onLanguageChange = (language: string) => {
  selectedLanguage.value = language;
  fetchRandomRepo();
};

// Función para refrescar el repositorio actual
const onRefresh = () => {
  console.log('Botón REFRESH presionado');
  fetchRandomRepo();
};

// Función para reintentar la carga en caso de error
const onRetry = () => {
  console.log('Botón RETRY presionado');
  fetchRandomRepo();
};

// Función para obtener un repositorio aleatorio basado en el lenguaje seleccionado
const fetchRandomRepo = async () => {
  if (!selectedLanguage.value) return;

  loading.value = true;
  error.value = false;
  repo.value = null;

  try {
    const token = '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'; 
    const response = await axios.get(
      `https://api.github.com/search/repositories?q=language:${selectedLanguage.value}&sort=stars&order=desc`,
      {
        headers: {
          Authorization: `token ${token}`,
        },
      }
    );

    const repos = response.data.items;
    if (repos.length > 0) {
      repo.value = repos[Math.floor(Math.random() * repos.length)];
    }
  } catch (err) {
    error.value = true;
  } finally {
    loading.value = false;
  }
};
</script>
