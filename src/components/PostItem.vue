<script setup>
  import { SIDEBAR_MODES } from '@/utils/sidebarModes';
  import { computed } from 'vue';

  const { post } = defineProps({
    post: Object,
  });

  const currentPostId = defineModel('currentPostId');
  const sidebarMode = defineModel('sidebarMode');

  const isCurrentPost = computed(() => currentPostId.value === post.id);

  const switchPost = () => {
    if (isCurrentPost.value) {
      currentPostId.value = null;
      sidebarMode.value = SIDEBAR_MODES.NONE

      return;
    }

    currentPostId.value = post.id;
    sidebarMode.value = SIDEBAR_MODES.POST;
  };
</script>

<template>
  <tr>
    <td>{{ post.id }}</td>

    <td>{{ post.title }}</td>

    <td class="has-text-right is-vcentered">
      <button
        type="button"
        :class="{ 'is-light': isCurrentPost }"
        class="button is-link"
        @click="switchPost"
      >
        {{ isCurrentPost ? 'Close' : 'Open' }}
      </button>
    </td>
  </tr>
</template>
