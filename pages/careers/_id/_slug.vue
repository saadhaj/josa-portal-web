<template>
  <careerSingle :career="career" />
</template>

<script>
  import axios from 'axios';
  import careerSingle from '~/components/Careers/CareerSingle';
  export default {
    layout: "default",
    asyncData(context) {
      return axios.get(process.env.baseUrl + '/careers/' + context.params.id)
        .then(res => {
          return {
            career: res.data
          }
        })
        .catch(e => context.error(e))
    },
    components: {
      careerSingle
    },
    head() {
      const i18nSeo = this.$nuxtI18nSeo()
      return {
        title: this.pageTitle + ' - ' + (this.$i18n.locale == 'ar' ? 'الجمعية الأردنية للمصدر المفتوح': 'Jordan Open Source Association'),
        meta: [{
            hid: 'career',
            name: 'Career'
          },
          ...this.$options.filters.ogTags('career', this.career, this.$route.path, this.$i18n.locale),
          ...i18nSeo.meta
        ]
      }
    },
    computed: {
      pageTitle() {
        return this.career['title_' + this.$i18n.locale]
      }
    },
  };

</script>
