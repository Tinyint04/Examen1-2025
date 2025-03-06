<template>
    <ion-item>
      <ion-label>Select a Language</ion-label>
      <ion-select v-model="selectedLanguage" @ionChange="onLanguageChange">
        <ion-select-option v-for="language in languages" :key="language" :value="language">
          {{ language }}
        </ion-select-option>
      </ion-select>
    </ion-item>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  import { IonItem, IonLabel, IonSelect, IonSelectOption } from '@ionic/vue';
  
  const selectedLanguage = ref('');
  const languages = ['JavaScript', 'Python', 'Java', 'TypeScript', 'Ruby', 'C#', 'PHP', 'C++', 'C', 'Swift'];
  
  const emit = defineEmits(['language-change']);
  
  const onLanguageChange = () => {
    emit('language-change', selectedLanguage.value);
  };
  </script>