<script setup>
  import { onMounted, ref } from 'vue';
  import NewPostForm from './NewPostForm.vue';
  import PostsList from './PostsList.vue';
  import Sidebar from './Sidebar.vue';
  import { getPosts } from '../api/posts';
  import Post from './Post.vue';
  import Header from './Header.vue';
  import { SIDEBAR_MODES } from '../utils/sidebarModes';
  import EditForm from './EditForm.vue';

  const user = defineModel('user');
  const sidebarMode = ref(SIDEBAR_MODES.NONE);
  const currentPostId = ref(null);
  const title = ref('');
  const body = ref('');
  const posts = ref([]);

  onMounted(async () => {
    try {
      posts.value = await getPosts();
    } catch (error) {
      console.error(error);
    }
  });
</script>

<template>
  <Header v-model:user="user" />

  <main class="section">
    <div class="container">
      <div class="tile ancestor">
        <PostsList
          :posts="posts"
          v-model:title="title"
          v-model:body="body"
          v-model:sidebarMode="sidebarMode"
          v-model:currentPostId="currentPostId"
        />

        <Sidebar :class="{ 'Sidebar--open': !!sidebarMode }">
          <Post
            v-if="sidebarMode === SIDEBAR_MODES.POST"
            :post-id="currentPostId"
            v-model:posts="posts"
            v-model:sidebarMode="sidebarMode"
            v-model:currentPostId="currentPostId"
            v-model:title="title"
            v-model:body="body"
          />

          <NewPostForm
            v-else-if="sidebarMode === SIDEBAR_MODES.NEW_POST_FORM"
            v-model:currentPostId="currentPostId"
            v-model:sidebarMode="sidebarMode"
            v-model:posts="posts"
            v-model:title="title"
            v-model:body="body"
          />

          <EditForm
            v-else-if="sidebarMode === SIDEBAR_MODES.EDIT_FORM"
            :currentPostId="currentPostId"
            v-model:sidebarMode="sidebarMode"
            v-model:posts="posts"
            v-model:title="title"
            v-model:body="body"
          />
        </Sidebar>
      </div>
    </div>
  </main>
</template>
