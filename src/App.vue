<template>
  <div class="ms-3 mt-3">
    スプレッドシート<br />
    <a href="https://docs.google.com/spreadsheets/d/1AB2-Z7k3JQf8yGnk74SofcnXCDQVowOCXNKqqoPVGus/edit#gid=508960181">https://docs.google.com/spreadsheets/d/1AB2-Z7k3JQf8yGnk74SofcnXCDQVowOCXNKqqoPVGus/edit#gid=508960181</a>
  </div>

  <div class="d-flex justify-content-center align-items-center" style="height: 100vh">
    <div class="card w-50 h-50" v-if="page === 1">
      <div class="card-header">① 出席確認</div>
      <div class="card-body">
        <p>
          出席確認をしましょう<br>
          欠席者がいる場合は下記のシートに⚪︎を入れてください<br>
          <a href="https://docs.google.com/spreadsheets/d/1SunBnF6VW01kVOve5EFyxIBU69G0cMOetKqyUcGXVGw/edit#gid=0" target="_blank">https://docs.google.com/spreadsheets/d/1SunBnF6VW01kVOve5EFyxIBU69G0cMOetKqyUcGXVGw/edit#gid=0</a>
        </p>
      </div>
      <div class="card-footer text-end">
        <button @click="prevPage" type="button" class="btn">前のページ</button>
        <button @click="nextPage" type="button" class="btn">次のページ</button>
      </div>
    </div>
    <div class="card w-50 h-50" v-if="page === 2">
      <div class="card-header">② 当日期限のチケット確認</div>
      <div class="card-body">
        <p>
          <a :href="today_tasks_url" target="_blank">チケットURL</a><br>
          ステータスが未対応状態なチケットはないかや未対応の場合はキャッチアップできているかの確認をすることで対応漏れを防ぐ
        </p>
      </div>
      <div class="card-footer text-end">
        <button @click="prevPage" type="button" class="btn">前のページ</button>
        <button @click="nextPage" type="button" class="btn">次のページ</button>
      </div>
    </div>
    <div class="card w-50 h-50" v-if="page === 3">
      <div class="card-header">③ 期限切れのチケット確認</div>
      <div class="card-body">
        <p>
          <a :href="overdue_tasks_url" target="_blank">チケットURL</a><br>
          スケジュールや先方を待たせていないかなどの対応状況に問題ないかの確認をすることで対応漏れの防止やスケジュール調整をする
        </p>
      </div>
      <div class="card-footer text-end">
        <button @click="prevPage" type="button" class="btn">前のページ</button>
        <button @click="nextPage" type="button" class="btn">次のページ</button>
      </div>
    </div>
    <div class="card w-50 h-50" v-if="page === 4">
      <div class="card-header">④ 前営業日作成のチケット確認</div>
      <div class="card-body">
        <p>
          <a :href="previous_business_day_tasks_url" target="_blank">チケットURL</a><br>
          お客様起票のチケットがないかや優先度が高となっているチケットがあれば担当者がキャッチアップできているか、担当者や期限日が設定されているかの確認をすることでチケット管理できるようにする        </p>
      </div>
      <div class="card-footer text-end">
        <button @click="prevPage" type="button" class="btn">前のページ</button>
        <button @click="nextPage" type="button" class="btn">次のページ</button>
      </div>
    </div>
    <div class="card w-50 h-50" v-if="page === 5">
      <div class="card-header">⑤ 処理済みのチケット確認</div>
      <div class="card-body">
        <p>月曜のみ確認</p>
        <p>
          <a :href="complete_tasks_url" target="_blank">チケットURL</a><br>
          1ヶ月以上残っているチケットはお客様起票の場合は先方が忘れている可能性があるため打診する必要があるかの確認やお客様起票以外の場合は完了にできるチケットはないかの確認をすることで放置チケットが無いようにする
        </p>
      </div>
      <div class="card-footer text-end">
        <button @click="prevPage" type="button" class="btn">前のページ</button>
        <button @click="nextPage" type="button" class="btn">次のページ</button>
      </div>
    </div>
    <div class="card w-50 h-50" v-if="page === 6">
      <div class="card-header">⑥ 全体確認</div>
      <div class="card-body">
        <p>
          健康診断<br>
          <a href="https://docs.google.com/spreadsheets/d/1dtpwrU48JVUT4PGYN0kellqaEkl8G6wG21Yh8Nnl2Vg/edit#gid=1366779378">https://docs.google.com/spreadsheets/d/1dtpwrU48JVUT4PGYN0kellqaEkl8G6wG21Yh8Nnl2Vg/edit#gid=1366779378</a>
        </p>
      </div>
      <div class="card-footer text-end">
        <button @click="prevPage" type="button" class="btn">前のページ</button>
        <button @click="nextPage" type="button" class="btn">次のページ</button>
      </div>
    </div>
    <div class="card w-50 h-50" v-if="page === 7">
      <div class="card-header">⑦ 運動</div>
      <div class="card-body">
      </div>
      <div class="card-footer text-end">
        <button @click="prevPage" type="button" class="btn">前のページ</button>
        <button @click="nextPage" type="button" class="btn">次のページ</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import dayjs from "dayjs";
import {ref} from "vue";

const page = ref(1)
const today_tasks_url = ref()
const overdue_tasks_url = ref()
const previous_business_day_tasks_url = ref()
const complete_tasks_url = ref()
const backlog_url = 'https://valeur.backlog.jp/FindIssueAllOver.action?sort=LIMIT_DATE&order=false&simpleSearch=false&allOver=true&startDate.unspecified=false&limitDate.unspecified=false&';
const base_api_sheet_url = 'https://sheets.googleapis.com/v4/spreadsheets/1AB2-Z7k3JQf8yGnk74SofcnXCDQVowOCXNKqqoPVGus/values/?sheet!?range?key=AIzaSyBhzOrlCWAfu2nBXzE6-YMbk4ktEE8Fbx4';

const generateGetSheetUrl = (sheet, range) => {
  return base_api_sheet_url.replace('?sheet', sheet).replace('?range', range)
}

const generateBaseBacklogUrl = async () => {
  const statusValues = (await axios.get(generateGetSheetUrl('URL', 'E2:G1000'))).data.values
  const projectValues = (await axios.get(generateGetSheetUrl('URL', 'A2:C1000'))).data.values
  const statusIds = []
  const projectIds = []
  const completeStatusIds = []

  for (const status of statusValues) {
    if (status[2] === '1') {
      completeStatusIds.push(status[0])
    } else {
      statusIds.push(status[0])
    }
  }

  for (const project of projectValues) {
    if (project[2] === '0') continue

    projectIds.push(project[0])
  }

  return {
    url: `${backlog_url}${projectIds.map(id => `projectId=${id}&`).join('')}${statusIds.map(id => `statusId=${id}&`).join('')}`,
    mondayUrl: `${backlog_url}${projectIds.map(id => `projectId=${id}&`).join('')}${completeStatusIds.map(id => `statusId=${id}&`).join('')}`
  }
}

const nextPage = () => {
  if (page.value === 7) {
    page.value = 1

    return
  }

  page.value = page.value + 1
}

const prevPage = () => {
  if (page.value === 1) {
    page.value = 7

    return
  }

  page.value = page.value - 1
}

(async () => {
  const base = await generateBaseBacklogUrl()
  const url = base.url
  const mondayUrl = base.mondayUrl
  const date = dayjs()
  const today = dayjs().format('YYYY/MM/DD')
  const yesterday = date.subtract(1, 'd').format('YYYY/MM/DD')
  const threeDaysAgo = date.subtract(3, 'd').format('YYYY/MM/DD')

  today_tasks_url.value = `${url}limitDateRange.begin=${today}&limitDateRange.end=${today}`
  overdue_tasks_url.value = `${url}limitDateRange.end=${yesterday}`
  previous_business_day_tasks_url.value = `${url}createdRange.begin=${dayjs().day() === 1 ? threeDaysAgo : yesterday}&createdRange.end=${today}`
  complete_tasks_url.value = mondayUrl
})()
</script>
