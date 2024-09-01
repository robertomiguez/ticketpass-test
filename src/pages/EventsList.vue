<template>
  <div>{{ events }}</div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { getEvents } from '@/services/EventService';
import type { Event as EventType } from '@/types/Event';

const events = ref<EventType[]>([]);
const fetchEvents = async () => {
  const fetchedEvents = await getEvents();
  events.value = fetchedEvents;
};

fetchEvents();
</script>
