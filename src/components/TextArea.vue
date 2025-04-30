<script setup>
  import { ERROR_MESSAGES } from '@/utils/errorMessages';

  defineProps({
    title: String,
    placeholder: String,
  });

  const error = defineModel('error');
  const model = defineModel();
</script>

<template>
  <div class="field" data-cy="BodyField">
    <label class="label" :for="title">{{ title }}</label>

    <div class="control">
      <textarea
        @input="error = ERROR_MESSAGES.NONE"
        v-model="model"
        :id="title"
        :name="title"
        :placeholder="placeholder"
        :class="{ 'is-danger': !!error }"
        class="textarea"
      ></textarea>
    </div>

    <p v-if="!!error" class="help is-danger" data-cy="ErrorMessage">{{ error }}</p>
  </div>
</template>
