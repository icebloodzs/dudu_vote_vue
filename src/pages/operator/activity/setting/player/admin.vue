<template>
  <div class="pb60">

    <base-navigation title="报名截止时间"></base-navigation>
    <group label-width="3.7rem" label-margin-right="2em" label-align="left">
      <datetime title="报名截止时间" class="color2" v-model="deadline" format="YYYY-MM-DD HH:mm"
                :minute-list="['00', '15', '30', '45']" year-row="{value}年" month-row="{value}月" day-row="{value}日"
                hour-row="{value}点" minute-row="{value}分" confirm-text="完成" cancel-text="取消"></datetime>
      <x-switch class="color2" title="关闭报名截止时间" :value-map="['0', '1']" v-model="is_close_deadline"></x-switch>
      <cell class="" inline-desc='温馨提示：选择设置报名表截止时间则表示报名截止时间前不能投票，不设置报名截止时间则表示可以边报名边投票'></cell>
    </group>
    <div class="plr20 pt30">
      <div @click="onSubmit" class="size26 color1 plr30 ptb15 border-radius5 text-center bg-29d6bf">保存</div>
    </div>
  </div>
</template>

<script>
  import { api } from 'h5sdk'

  export default {
    data () {
      return {
        is_close_deadline: '1',
        deadline: ''
      }
    },
    async created () {
      this.fetchData()
    },
    methods: {
      async onSubmit () {
        let activityId = this.$route.params.activity_id
        const {data} = await api.post('operator_activity_config', {
          id: activityId,
          is_close_deadline: this.is_close_deadline,
          deadline: this.deadline,
        })

        this.$vux.toast.show({
          text: data.message,
        })
      },
      async fetchData () {
        let activityId = this.$route.params.activity_id

        const {data} = await api.get('operator_activity', {id: activityId})

        console.log(data)
        this.is_close_deadline = String(data.data.is_close_deadline)
        this.deadline = data.data.deadline
      }
    }
  }
</script>

<style lang="scss" scoped>
</style>
