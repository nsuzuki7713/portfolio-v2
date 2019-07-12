<template>
  <!-- <v-layout column justify-center align-center> -->
  <v-container grid-list-md text-xs-center>
    <v-layout row grid-list-md text-xs-center wrap>
      <v-flex xs4>
        <v-card>
          <v-avatar :tile="tile" size="200">
            <img src="~/assets/icon.JPG" alt="avatar" />
          </v-avatar>
        </v-card>
      </v-flex>
      <v-flex xs8>
        <v-card>
          <v-card-text class="px-0">
            <h2>
              基本情報(<a @click="switchBasicInfoType(1)">JSON</a>/<a @click="switchBasicInfoType(2)">CSV</a>/<a
                @click="switchBasicInfoType(3)"
                >YAML</a
              >/<a @click="switchBasicInfoType(4)">Table</a>)
            </h2>
            <pre v-if="basicInfoType !== 4">
            <code class="text-sm-left" style="font-size:14px;width: 80%;background: white;" v-if="basicInfoType===1">
              body { 
                    名前: "なおと",
                    性別: "男性",
                    生年月日: "1993年",
                    生息地: "東京都",
                    区分: "個人事業主",
              }
            </code>
            <code class="text-sm-left" style="font-size:14px;width: 80%;background: white;" v-if="basicInfoType===2">
              "名前","性別","生年月日","生息地","区分"
              "なおと","男性","1993年","東京都","個人事業主"
            </code>
            <code class="text-sm-left" style="font-size:14px;width: 80%;background: white;" v-if="basicInfoType===3">
              body
                  名前: "なおと"
                  性別: "男性"
                  生年月日: "1993年"
                  生息地: "東京都"
                  区分: "個人事業主"
            </code>
          </pre>
            <div style="font-size:14px;width: 80%;background: white;margin:0 auto;" v-if="basicInfoType === 4">
              <v-data-table :headers="headers" :items="gitHubObj.basicInfo" hide-actions>
                <template v-slot:items="props">
                  <td class="text-xs-left">{{ props.item.key }}</td>
                  <td class="text-xs-left">{{ props.item.value }}</td>
                </template>
              </v-data-table>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>

      <v-flex xs3>
        <v-card style="height:370px">
          <img src="~/assets/GitHub-Mark-120px-plus.png" aspect-ratio="2.75" />
          <v-card-text class="px-1">
            <div>
              <div style="font-size: 24px;">contributions</div>
              <div style="font-size: 24px;">
                <animated-number :value="contributionsObj.total" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">直近1週間</div>
              <div style="font-size: 24px;">
                <animated-number :value="contributionsObj.week" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">昨日</div>
              <div style="font-size: 24px;">
                <animated-number :value="contributionsObj.yesterday" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>

      <v-flex xs3>
        <v-card style="height:370px">
          <img src="~/assets/qiita-rectangle.png" height="120px;" />
          <v-card-text class="px-0">
            <div>
              <div style="font-size: 24px;">投稿数</div>
              <div style="font-size: 24px;">
                <animated-number :value="qiitaObj.itemCount" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">contribution</div>
              <div style="font-size: 24px;">
                <animated-number :value="qiitaObj.contribution" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>

      <v-flex xs3>
        <v-card style="height:370px">
          <img src="~/assets/Twitter_Social_Icon_Circle_Color.png" height="120px;" />
          <v-card-text class="px-0">
            <div>
              <div style="font-size: 24px;">ツイート</div>
              <div style="font-size: 24px;">
                <animated-number :value="twitterObj.tweet" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">フォロー</div>
              <div style="font-size: 24px;">
                <animated-number :value="twitterObj.following" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">フォロワー</div>
              <div style="font-size: 24px;">
                <animated-number :value="twitterObj.followers" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>

      <v-flex xs3>
        <v-card style="height:370px">
          <img src="~/assets/connpass_logo_3.png" height="120px;" />
          <v-card-text class="px-0">
            <div>
              <div style="font-size: 24px;">参加</div>
              <div style="font-size: 24px;">
                <animated-number :value="connpassObj.participation" :formatValue="formatToPrice" :duration="2000" />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">直近1ヶ月</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="connpassObj.participationMonth"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>

      <v-flex xs12>
        <v-card>
          業務経歴
          <v-card-text class="px-0"> </v-card-text>

          <div class="d-flex justify-between align-center mb-3">
            <v-btn @click="all">全て開く</v-btn>
            <v-btn @click="none">全て閉じる</v-btn>
          </div>

          <v-expansion-panel v-model="panel" expand>
            <v-expansion-panel-content v-for="(cvItems, i) in cvItems" :key="i">
              <template v-slot:header>
                <div style="font-size:16px;">
                  サーバレスで大量データ集積の開発【Vue/Node.js/TypeScript/AWS】(2019年:現在)
                </div>
              </template>
              <v-card>
                <v-card-text>
                  <h3>【プロジェクト概要】</h3>
                  <p>
                    サーバレス環境で大量データを集積するプロジェクトです。現在は主にVueでフロントの開発を行っています。
                  </p>
                  <h3>【担当業務】</h3>
                  <p>
                    Vueで業務系のWebシステム開発(新規)。使用言語はVue、TypeScript。
                  </p>
                </v-card-text>
              </v-card>
            </v-expansion-panel-content>
          </v-expansion-panel>
        </v-card>
      </v-flex>
    </v-layout>

    <!-- <v-card>
      <h1>githubから取得する職務経歴書など</h1>
      {{ gitHubCv }}
    </v-card> -->
    {{ gitHubObj }}
  </v-container>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'
import AnimatedNumber from 'animated-number-vue'
import cheerio from 'cheerio'

export default {
  components: {
    Logo,
    VuetifyLogo,
    AnimatedNumber
  },
  data: () => ({
    tile: false,
    basicInfoType: 1,
    value: 1900,
    headers: [{ text: 'key', value: 'key', sortable: false }, { text: 'value', value: 'value', sortable: false }],
    panel: [],
    cvItems: 5
  }),
  async asyncData({ app }) {
    const urlPairs = [
      ['https://qiita.com/turmericN', createQiitaObject],
      ['https://twitter.com/naoto_7713', createTwitterObject],
      ['https://github.com/users/nsuzuki7713/contributions', createGitHubContributionsObject],
      ['https://connpass.com/user/s_naoto/', createConnpassObject],
      ['https://github.com/nsuzuki7713/cv/blob/master/README.md', createGithubCvObject]
    ]

    const cb = ([url, convert]) => app.$axios.$get(url).then(convert)
    const [qiitaObj, twitterObj, contributionsObj, connpassObj, gitHubObj] = await Promise.all(urlPairs.map(cb))

    return { qiitaObj, twitterObj, contributionsObj, connpassObj, gitHubObj }
  },
  methods: {
    switchBasicInfoType(type) {
      this.basicInfoType = type
    },
    formatToPrice(value) {
      return `${value.toFixed(0)}`
    },
    all() {
      this.panel = [...Array(this.cvItems).keys()].map(_ => true)
    },
    // Reset the panel
    none() {
      this.panel = []
    }
  }
}

function createQiitaObject(html) {
  const $ = cheerio.load(html)
  const qiita = {}
  qiita.itemCount = $('.userActivityChart_statCount')
    .eq(0)
    .text()
  qiita.contribution = $('.userActivityChart_statCount')
    .eq(1)
    .text()

  return qiita
}

function createTwitterObject(html) {
  const $ = cheerio.load(html)
  const twitter = {}
  twitter.tweet = $('.ProfileNav-value')
    .eq(0)
    .text()
    .replace(/\r?\n/g, '')
    .replace(',', '')

  twitter.following = $('.ProfileNav-value')
    .eq(1)
    .text()

  twitter.followers = $('.ProfileNav-value')
    .eq(2)
    .text()

  return twitter
}

function createGitHubContributionsObject(html) {
  const $ = cheerio.load(html)
  const cb = {}
  // 総計のContributions
  cb.total = $('h2')
    .eq(0)
    .text()
    .trim()
    .split(' ')[0]

  // 昨日のContributions
  cb.yesterday = $('.day')
    .last()
    .prev()
    .data('count')

  // 今週のContributions(日〜土)
  let weekCnt = 0
  let targetDay = $('.day').last()
  for (let i = 0; i < 7; i++) {
    weekCnt += targetDay.data('count')
    targetDay = targetDay.prev()

    if (targetDay.data('count') === undefined) {
      break
    }
  }
  cb.week = weekCnt

  return cb
}

function createConnpassObject(html) {
  const $ = cheerio.load(html)
  const connpass = {}

  // 参加
  connpass.participation = $('.num')
    .eq(0)
    .text()

  let participationMonth = 0
  const beforeMonth = new Date().setMonth(new Date().getMonth() - 1)
  $('.event_schedule_area').each((i, elem) => {
    const eventDate = new Date(
      $(elem)
        .find($('.year'))
        .text() +
        '/' +
        $(elem)
          .find($('.date'))
          .text()
    )

    if (beforeMonth <= eventDate && eventDate <= new Date()) {
      participationMonth++
    }
  })

  connpass.participationMonth = participationMonth

  return connpass
}

function createGithubCvObject(html) {
  const $ = cheerio.load(html)
  const cv = {}

  let basicInfo = []
  $('tbody tr').each((i, elem) => {
    const targetTd = $(elem).find('td')
    const obj = {}
    obj.key = targetTd.eq(0).text()
    obj.value = targetTd.eq(1).text()
    basicInfo.push(obj)
  })

  cv.basicInfo = basicInfo
  return cv
}
</script>
