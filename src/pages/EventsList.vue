<template>
  <EventCard
    v-for="event in events"
    :key="event.id"
    :event="event"
    class="p-1"
  />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { getEvents } from '@/services/EventService';
import type { Event as EventType } from '@/types/Event';
import EventCard from '@/components/EventCard.vue';

const events = ref<EventType[]>([]);
const fetchEvents = async () => {
  const fetchedEvents = await getEvents();
  events.value = fetchedEvents;
};

fetchEvents();
</script>
