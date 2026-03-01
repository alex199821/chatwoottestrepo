<script setup>
import { computed } from 'vue';
import ChatCard from 'shared/components/ChatCard.vue';
import MessageMeta from '../MessageMeta.vue';
import { useMessageContext } from '../provider.js';
import { ORIENTATION } from '../constants.js';

const { contentAttributes, shouldGroupWithNext, orientation } =
  useMessageContext();

const items = computed(() => contentAttributes.items ?? []);

const flexOrientationClass = computed(() => {
  const map = {
    [ORIENTATION.LEFT]: 'justify-start',
    [ORIENTATION.RIGHT]: 'justify-end',
    [ORIENTATION.CENTER]: 'justify-center',
  };
  return map[orientation.value];
});

console.log(contentAttributes);
const combinedTitles = computed(() => {
  return items.value.map(item => item.title).join(', ');
});

console.log("titles are:", combinedTitles);
</script>

<template>
  <h1>{{ combinedTitles }}</h1>
  <div class="flex flex-col gap-1">
    <div class="flex flex-wrap gap-2" :class="flexOrientationClass">
      <ChatCard v-for="item in items" :key="item.title" :media-url="item.mediaUrl" :title="item.title"
        :description="item.description" :actions="item.actions" />
    </div>
    <MessageMeta v-if="!shouldGroupWithNext" class="mt-1 text-n-slate-11" :class="flexOrientationClass" />
  </div>
</template>
