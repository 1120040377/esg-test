<template>
  <n-card :title="qno + '. ' + q?.title" class="question">
    <div>
      <div>{{ mode }}
        <div v-for="( option, index ) in  q.options " :key="index" class="question-option"
          :class="{ [mode]: true, 'is-right': option.indexOf(q.answer) === 0 && mode === 'wrong' }"
          @click="actionChecked(q, index)">
          <n-checkbox size="large" :checked="q.checked === index" :disabled="mode === 'wrong'" />
          {{ option }}
        </div>
      </div>
      <div class="question-tips">说明：{{ q.description }}</div>
    </div>
  </n-card>

</template>

<script setup>
import { computed } from 'vue';
const noList = ['A', 'B', 'C', 'D']
const props = defineProps({
  data: {
    type: Object,
    default: () => {
      return {}
    }
  },
  qno: {
    type: Number,
  },
  mode: {
    type: String,
    default: 'normal'
  }
})

const q = computed(() => {
  return props.data
})
function actionChecked(q, index) {
  if (props.mode === 'wrong') {
    return
  }
  q.checked = index
}
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

    &.is-right {
      color: #FFF;
      background: orange;

      &::before {
        content: '正确答案';
        margin-right: 10px;
      }
    }

    &.normal:hover {
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
