<template>
  <span @click="clickVote">
      <slot></slot>
    <!--投票成功-->
      <x-dialog :show.sync="showDialog" :hide-on-blur="true" :dialog-style="{width: '100%'}">
        <div class="pt30 plr30">
          <div class="flex-wrp flex-center border-b pb20">
            <div class="" :style="{width: rem(50), height: rem(50)}">
              <svg class="icon base-menu-icon" aria-hidden="true">
                <use :xlink:href="`#icon-chenggong`"></use>
              </svg>
            </div>
            <div class="color2 size32 pl20">投票成功</div>
          </div>
          <div class="color5 size22 ptb50">好友也可以帮忙，一天后还能再投票哦！</div>

          <router-link :to="`/activity/${activity.info.id}/player/${player.id}/gift`">
          <div class="ptb15 bg-ff404b size26 color1 border-radius5">给TA送礼物加票</div>
          </router-link>

          <div class="flex-wrp flex-center" @click.stop="showDialog=false">
            <div class="guanbi border-radius">
              <svg class="icon base-menu-icon" aria-hidden="true" :style="{width: rem(52), height: rem(80)}">
                <use :xlink:href="`#icon-shanchu4-copy`"></use>
              </svg>
            </div>
          </div>
        </div>
      </x-dialog>
  </span>
</template>

<script>
  import { api } from 'h5sdk'
  import { createNamespacedHelpers } from 'vuex'

  const {mapState} = createNamespacedHelpers('activity')

  export default {
    props: [
      'children',
      'player',
    ],
    data () {
      return {
        showDialog: false,
      }
    },
    computed: {
      ...mapState([
        'activity',
      ]),
    },
    methods: {
      async clickVote () { // 投普通票
        const {data} = await api.get('activity_voting', {
          player_id: this.player.id,
        })
        if (data.error) {
          alert(data.message)
        } else {
          this.showDialog = true
          this.$emit('on-voting', 1)
        }
      },
    },
  }
</script>

<style lang="scss" scoped>
  .guanbi {
    // position: absolute;
    // top: -10px;
    // right: -10px;
    padding: 10px 20px;
    // z-index: 999;
  }
</style>
