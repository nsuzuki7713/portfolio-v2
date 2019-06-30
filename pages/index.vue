<template>
  <!-- <v-layout column justify-center align-center> -->
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
      <v-card dark color="secondary">
        <v-card-text class="px-0">
          <div>GitHub</div>
          <div>contributions:700</div>
          <div>直近一週間:700</div>
          <div>{{ items.results_returned }}</div>
        </v-card-text>
      </v-card>
    </v-flex>
    <v-flex xs3>
      <v-card dark color="secondary">
        <v-card-text class="px-0">Qiita</v-card-text>
      </v-card>
    </v-flex>
    <v-flex xs3>
      <v-card dark color="secondary">
        <v-card-text class="px-0">SlidShare/speakerdeck</v-card-text>
      </v-card>
    </v-flex>
    <v-flex xs3>
      <v-card dark color="secondary">
        <v-card-text class="px-0">Twitter</v-card-text>
      </v-card>
    </v-flex>

    <!-- <v-flex xs12 sm8 md6>
      <div class="text-xs-center">
        <logo />
        <vuetify-logo />
      </div>
      <v-card>
        <v-card-title class="headline"
          >Welcome to the Vuetify + Nuxt.js template</v-card-title
        >
        <v-card-text>
          <p>
            Vuetify is a progressive Material Design component framework for
            Vue.js. It was designed to empower developers to create amazing
            applications.
          </p>
          <p>
            For more information on Vuetify, check out the
            <a href="https://vuetifyjs.com" target="_blank">documentation</a>.
          </p>
          <p>
            If you have questions, please join the official
            <a href="https://chat.vuetifyjs.com/" target="_blank" title="chat"
              >discord</a
            >.
          </p>
          <p>
            Find a bug? Report it on the github
            <a
              href="https://github.com/vuetifyjs/vuetify/issues"
              target="_blank"
              title="contribute"
              >issue board</a
            >.
          </p>
          <p>
            Thank you for developing with Vuetify and I look forward to bringing
            more exciting features in the future.
          </p>
          <div class="text-xs-right">
            <em><small>&mdash; John Leider</small></em>
          </div>
          <hr class="my-3" />
          <a href="https://nuxtjs.org/" target="_blank">Nuxt Documentation</a>
          <br />
          <a href="https://github.com/nuxt/nuxt.js" target="_blank"
            >Nuxt GitHub</a
          >
        </v-card-text>
        <v-card-actions>
          <v-spacer />
          <v-btn color="primary" flat nuxt to="/inspire">Continue</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex> -->
  </v-layout>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data: () => ({
    tile: false,
    basicInfoType: 1,
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
    }
  }
}
</script>
