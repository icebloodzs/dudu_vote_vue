<template>
  <div class="pb60">

    <base-navigation title="添加排名奖"></base-navigation>

    <group label-width="2.1rem" label-margin-right="2em" label-align="left" gutter=".3rem">
      <x-input title="奖品名称:" placeholder="请输入奖品名称" required v-model="prize.name"/>
      <cell title="奖品详情"/>
    </group>

    <InputRichText class="bg-white" v-model="prize.details"/>


    <div class="plr20 ptb10">
      <div class="size26 color1 plr30 ptb15 border-radius5 text-center bg-29d6bf" @click="onSubmit">保存</div>
    </div>
  </div>
</template>

<script>
  import { api } from 'h5sdk'

  import InputRichText from '@/components/input/rich-text/InputRichText'

  export default {
    components: {
      InputRichText
    },
    data () {
      return {
        prize: {
          name: '',
          details: [],
          type: 0,
        },
      }
    },
    async created () {
      this.fetchData()
    },

    methods: {
      async fetchData () {
        const {data} = await api.get('operator_prize', {
          prize_id: this.$route.params.prize_id,
        })
        this.prize.id = this.$route.params.prize_id
        this.prize.name = data.data.name
        this.prize.details = data.data.details
      },
      async onSubmit () {
        let tes = this.prize.details.find((item) => {
          return item.image !== undefined
        })

        if (tes === undefined) {
          this.$vux.toast.text('请至少添加一张图片', 'middle')
          return
        }

        let requestData = this.prize
        const {data} = await api.post('operator_prize_update', requestData)
        this.$vux.toast.text(data.message, 'middle')
        if (!data.error) {
          this.$router.back()
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
</style>
