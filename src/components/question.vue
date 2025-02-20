<template>
  <n-card :title="q?.title" class="question">
    <div>
      <div>
        <div v-for="( option, index ) in  q.options " :key="index" class="question-option" @click="q.checked = index">
          <n-checkbox size="large" :checked="q.checked === index" />
          {{ option }}
        </div>
      </div>
      <div>参考答案：{{ q.answer.join(',') }}</div>
      <div>说明：{{ q.description }}</div>
    </div>
  </n-card>

</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  data: {
    type: Object,
    default: () => {
      return {}
    }
  }
})

const q = computed(() => {
  return props.data
})

</script>

<style scoped lang="scss">
.question {
  margin: 16px 0;

  &-option {
    font-size: 14px;
    padding: 5px 0;
    cursor: pointer;
  }

}
</style>
