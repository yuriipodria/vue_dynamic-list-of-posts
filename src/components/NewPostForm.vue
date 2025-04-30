<script setup>
  import { SIDEBAR_MODES } from '@/utils/sidebarModes';
  import { createPost } from '@/api/posts';
  import PostForm from './PostForm.vue';

  const posts = defineModel('posts');
  const sidebarMode = defineModel('sidebarMode');
  const currentPostId = defineModel('currentPostId');
  const title = defineModel('title');
  const body = defineModel('body');

  const onSubmit = async () => {
    try {
      const newPost = await createPost(title.value.trim(), body.value.trim());

      posts.value.push(newPost);

      sidebarMode.value = SIDEBAR_MODES.POST;
      currentPostId.value = newPost.id;

      title.value = '';
      body.value = '';
    } catch (error) {
      console.error(error);
    }
  };

  const onCancel = async () => {
    sidebarMode.value = SIDEBAR_MODES.NONE;
  };
</script>

<template>
  <div class="content">
    <h2>Create new post</h2>

    <PostForm :onSubmit="onSubmit" :onCancel="onCancel" v-model:title="title" v-model:body="body" />
  </div>
</template>
