<template>
  <div>
    <div
      class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-x-4 lg:pl-42 lg:pr-42"
    >
      <EventCard
        v-for="event in paginatedEvents"
        :key="event.id"
        :event="event"
        class="p-1"
      />
    </div>
    <hr class="my-4 border-t border-gray-300" />
    <Pagination
      :totalPages="totalPages"
      :currentPage="currentPage"
      @change-page="currentPage = $event"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import EventCard from '@/components/EventCard.vue';
import Pagination from '@/components/PaginationEvent.vue';
import { getEvents } from '@/services/EventService';
import type { Event as EventType } from '@/types/Event';

const events = ref<EventType[]>([]);
const currentPage = ref(1);
const eventsPerPage = 8;

// function to assign prices to events
// price usually comes from the backend
const assignPricesToEvents = (eventsData: EventType[]) => {
  return eventsData.map(event => ({
    ...event,
    price: event.price || Math.floor(Math.random() * (500 - 20 + 1)) + 20,
  }));
};

const fetchEvents = async () => {
  const fetchedEvents = await getEvents();
  events.value = assignPricesToEvents(fetchedEvents);
};

fetchEvents();

const paginatedEvents = computed(() => {
  const start = (currentPage.value - 1) * eventsPerPage;
  const end = start + eventsPerPage;
  return events.value.slice(start, end);
});

const totalPages = computed(() =>
  Math.ceil(events.value.length / eventsPerPage),
);
</script>
