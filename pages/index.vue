<template>
  <div class="container">
    <div>
      <select @change="foo($event)" name="language-picker-select" id="language-picker-select">
        <option value="en-gb" selected>English</option>
        <option value="es-es">Español</option>
        <option value="zh-cn">简体中文</option>
        <!-- other language options -->
      </select>
      <h1 class="title">
        {{ introText }}
      </h1>

    </div>
  </div>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  async asyncData(context) {
      const {error, $prismic} = context
      let introductionContent = {}
      try {
        // Query to get introduction page content
        /*
        const foo = await axios.get('https://jsonplaceholder.typicode.com/posts')
        console.log(foo.data)
        return {introductionContent: foo.data}
        */
        let fooBar = await $prismic.api.query(
          $prismic.predicates.at('document.type', 'introduction'),
          {lang: '*'})
        fooBar = fooBar.results
        const langMap = fooBar.map((f) => {return {"lang": f.lang, "data": f.data}})
       /* fooBar = fooBar.map(r => {
          fooBar["foo"] = r.data;
        }) */
        return {langMap}
      } catch (e) {
        // Returns error page
        console.log(e)
        error({statusCode: 404, message: 'Page not found'})
      }

  },
  beforeCreate() {
    //debugger;
  },
  data() {
    return {
      //introductionContent: [],
      selectedLanguage: "en-gb",
      dataReady: false
    }
  },
  created(context) {

  },
  computed: {
    introText() {
      debugger;
      const selectedLanguageContent =  this.langMap.find(f => f.lang === this.selectedLanguage)
      const text = this.$prismic.asText(selectedLanguageContent.data.welcome)
      return text
    }
  },
  mounted() {
    this.isMounted = true;
  },

  methods: {
    foo(event) {
      this.selectedLanguage = event.target.value
    },

  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
