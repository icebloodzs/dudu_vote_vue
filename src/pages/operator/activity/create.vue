<template>
    <div>

        <ActivityThemeDefault :activity="activity" :publish="publish"/>
        <divider>我是有底线的</divider>

    </div>
</template>

<script>
    import InputText from '@/components/input/InputText'
    import ActivityEditorSwiper from '@/components/input/swiper/InputSwiper'
    import ActivityEditorContent from '@/components/input/rich-text/InputRichText'
    import ActivityThemeDefault from '@/components/operator/activity/themes/OperatorActivityThemesDefault'
    import { api } from 'h5sdk'

    export default {
      components: {
        InputText,
        ActivityEditorSwiper,
        ActivityEditorContent,
        ActivityThemeDefault
      },
      data () {
        return {
          activity: {
            swiper: [],
            title: '最美女神投票活动',
            contentTitle: '活动介绍',
            content: [
              {
                text: '本活动主要为大家提供娱乐有趣的活动，增加生活情调，享受生活美好，不会存在任何欺骗、诈骗，本平台也不会以任何中奖名义要求转账或者其他欺骗行为。'
              }
            ],
            startTime: '',
            endTime: '',
            signUpManner: 0,
            voteType: 0,
            nextVoteTime: 0,
            limitedExchangeCount: 0
          }
        }
      },
      methods: {
        async publish (activity) {
          const requestData = {
            'sliders': activity.swiper,
            'title': activity.title,
            'content_title': activity.contentTitle,
            'content': activity.content,
            'start_time': activity.startTime,
            'end_time': activity.endTime,
            'sign_up_manner': activity.signUpManner,
            'vote_type': activity.voteType,
            'next_vote_time': activity.nextVoteTime,
            'limited_exchange_count': activity.limitedExchangeCount
          }

          const {data} = await api.post('operator_activity_create', requestData)

          alert(data.message)

          if (data.error) {
          } else {
            this.$router.push('/operator')
          }
        }
      }
    }
</script>

<style scoped>
    button {
        border: 1px solid;
    }
</style>
