<script setup>
  import { ref } from 'vue';
  import Input from './Input.vue';
  import TextArea from './TextArea.vue';
  import { ERROR_MESSAGES } from '@/utils/errorMessages';

  const INITIAL_ERRORS = {
    titleError: ERROR_MESSAGES.NONE,
    bodyError: ERROR_MESSAGES.NONE,
  };

  const { onSubmit } = defineProps({
    onSubmit: Function,
    onCancel: Function,
  });

  const title = defineModel('title');
  const body = defineModel('body');
  const errors = ref({ ...INITIAL_ERRORS });

  const onFormSubmit = async () => {
    errors.value = { ...INITIAL_ERRORS };

    if (!title.value.trim()) {
      errors.value.titleError = ERROR_MESSAGES.TITLE_REQUIRED;
    }

    if (!body.value.trim()) {
      errors.value.bodyError = ERROR_MESSAGES.BODY_REQUIRED;
    }

    if (errors.value.titleError || errors.value.bodyError) {
      return;
    }

    await onSubmit();
  };
</script>

<template>
  <form @submit.prevent="onFormSubmit">
    <Input
      v-model="title"
      title="Title"
      placeholder="New title"
      v-model:error="errors.titleError"
      icon="fa-user"
    />
    <TextArea
      v-model="body"
      title="Write Post Body"
      placeholder="Post body"
      v-model:error="errors.bodyError"
    />

    <div class="field is-grouped">
      <div class="control">
        <button type="submit" class="button is-link">Save</button>
      </div>

      <div class="control">
        <button type="reset" class="button is-link is-light" @click="onCancel">Cancel</button>
      </div>
    </div>
  </form>
</template>
