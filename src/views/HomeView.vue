<template>
  <div class="page">
    <div class="page-header">
      <n-button @click="reloadPage">重置</n-button>
      <n-button style="margin-left: 12px" type="primary" @click="actionSubmit">提交</n-button>
    </div>
    <n-modal v-model:show="showModal">
      <n-card style="width: 900px" title="选择的答案" :bordered="false" size="huge" role="dialog" aria-modal="true">
        <template #header-extra>
          <div style="cursor: pointer;" @click="showModal = false">关闭</div>
        </template>
        <div>
          <div>总分：{{ source }}</div>
          <!-- <n-button>查看错题</n-button> -->
        </div>
        <div v-if="answerList.length > 0">
          <div v-for="row in answerList">
            <n-tag v-for="col in row" type="info" class="col-item">{{ col.no.padStart(3, '&ensp;') }}： <b>{{ col.val
                }}</b></n-tag>
          </div>
        </div>
      </n-card>
    </n-modal>

    <div class="page-view">
      <Question v-for="(item, index) in qnList" :qno="index + 1" :data="item" :key="item.questionId"></Question>

    </div>

  </div>
</template>

<script setup>
import Question from '@/components/question.vue'
// import QN from '@/assets/data.js'
import QN from '@/assets/data3.js'
import { ref } from 'vue';
const qnList = ref([])
const showModal = ref(false)
const source = ref(0)
reloadPage()
function reloadPage() {
  qnList.value = []
  QN.forEach(item => {
    qnList.value.push({
      questionId: Math.random().toString(36).substr(2),
      checked: null,
      ...item
    })
  })
  shuffleArray(qnList.value)
}

function shuffleArray(array) {
  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
}

const answerList = ref([])

function actionSubmit() {
  source.value = 0
  const noList = ['A', 'B', 'C', 'D']
  const arr = []
  let curArr = []
  const qns = JSON.parse(JSON.stringify(qnList.value))
  qns.sort((a, b) => a.no - b.no)
  qns.forEach((item, index) => {
    const data = {
      no: item.no,
      val: noList[item.checked] || '-',
      isRight: noList[item.checked] === item.answer
    }
    if (index < 2) {
      console.log(data, item)
    }
    if (data.isRight) {
      source.value += 1
    }
    curArr.push(data)

    if (index % 10 === 9) {
      arr.push(curArr)
      curArr = []
    }
  })
  answerList.value = arr
  showModal.value = true
}
</script>

<style scoped lang="scss">
.page {
  min-height: 100vh;
  background: #efefef;
  padding-top: 60px;

  &-header {
    z-index: 10;
    height: 60px;
    background-color: #FFF;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    padding: 0 16px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }

  &-view {
    width: 968px;
    margin: 0 auto
  }
}

.col-item {
  width: 72px;
  margin: 5px;
}
</style>
