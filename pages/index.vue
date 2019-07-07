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
              基本情報(<a @click="switchBasicInfoType(1)">JSON</a>/<a
                @click="switchBasicInfoType(2)"
                >CSV</a
              >/<a @click="switchBasicInfoType(3)">YAML</a>/<a
                @click="switchBasicInfoType(4)"
                >Table</a
              >)
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
            <div
              style="font-size:14px;width: 80%;background: white;margin:0 auto;"
              v-if="basicInfoType === 4"
            >
              <v-data-table :headers="headers" :items="desserts" hide-actions>
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
                <animated-number
                  :value="32"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">直近1週間</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="700"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">昨日</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="2"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
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
                <animated-number
                  :value="55"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">contribution</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="1915"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
            <div style="display:none">
              <div style="font-size: 24px;">contribution</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="1915"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>

      <v-flex xs3>
        <v-card style="height:370px">
          <img
            src="~/assets/Twitter_Social_Icon_Circle_Color.png"
            height="120px;"
          />
          <v-card-text class="px-0">
            <div>
              <div style="font-size: 24px;">ツイート</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="3803"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">フォロー</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="766"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">フォロワー</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="843"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
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
                <animated-number
                  :value="38"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
            <div>
              <div style="font-size: 24px;">直近1ヶ月</div>
              <div style="font-size: 24px;">
                <animated-number
                  :value="2"
                  :formatValue="formatToPrice"
                  :duration="2000"
                />
              </div>
            </div>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'
import AnimatedNumber from 'animated-number-vue'

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
    headers: [
      { text: 'key', value: 'key', sortable: false },
      { text: 'value', value: 'value', sortable: false }
    ],
    desserts: [
      {
        key: '名前',
        value: 'なおと'
      },
      {
        key: '性別',
        value: '男性'
      },
      {
        key: '生年月日',
        value: 1993
      },
      {
        key: '生息地',
        value: '東京'
      },
      {
        key: '区分',
        value: '個人事業主'
      }
    ]
  }),
  async asyncData({ app }) {
    const items = await app.$axios.$get('https://connpass.com/api/v1/event/')
    return { items }
  },
  methods: {
    switchBasicInfoType(type) {
      this.basicInfoType = type
    },
    formatToPrice(value) {
      return `${value.toFixed(0)}`
    }
  }
}
</script>
