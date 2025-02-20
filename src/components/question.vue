<template>
  <n-card :title="q?.no + '. ' + q?.title" class="question">
    <div>
      <div>
        <div v-for="( option, index ) in  q.options " :key="index" class="question-option" @click="q.checked = index">
          <n-checkbox size="large" :checked="q.checked === index" />
          {{ option }}
        </div>
      </div>
      <div class="question-tips">说明：{{ q.description }}</div>
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
    padding: 5px 10px;
    cursor: pointer;
    margin-left: -10x;
    transition: all .3s linear;
    border-radius: 5px;

    &:hover {
      background: #3185f2;
      color: #FFF;
    }
  }

  &-tips {
    margin-top: 12px;
    color: #999;
    font-size: 12px;
  }

}
</style>
