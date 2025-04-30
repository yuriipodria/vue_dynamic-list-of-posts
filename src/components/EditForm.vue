<script setup>
  import { SIDEBAR_MODES } from '@/utils/sidebarModes';
  import PostForm from './PostForm.vue';
  import { editPost } from '@/api/posts';

  const { currentPostId } = defineProps({
    currentPostId: Number,
  });

  const posts = defineModel('posts');
  const sidebarMode = defineModel('sidebarMode');
  const title = defineModel('title');
  const body = defineModel('body');

  const onSubmit = async () => {
    try {
      const newPost = await editPost(currentPostId, title.value.trim(), body.value.trim());

      const index = posts.value.findIndex(item => item.id === currentPostId);
      posts.value[index] = newPost;

      sidebarMode.value = SIDEBAR_MODES.POST;
    } catch (error) {
      console.error(error);
    }
  };

  const onCancel = () => {
    sidebarMode.value = SIDEBAR_MODES.POST;
  };
</script>

<template>
  <div class="content">
    <h2>Edit post</h2>

    <PostForm :onSubmit="onSubmit" :onCancel="onCancel" v-model:title="title" v-model:body="body" />
  </div>
</template>
