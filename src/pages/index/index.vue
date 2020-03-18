<template>
  <div @click="clickHandle">
    <form class="form-container" v-if="userIsGet">
      <div class="userLogo">
        <div><img :src="userInfo.avatarUrl"></div>
        <div v-html="userInfo.nickName"></div>
      </div>

    </form>
    <form class="form-container" v-if="userIsGet==false">
      <div class="flex xc yc">
        <button open-type="getUserInfo" lang="zh_CN" @getuserinfo='initUserInfo'>未登陆，点击登陆</button>
      </div>
    </form>
  </div>
</template>

<script>
  import card from '@/components/card'

  export default {
    data () {
      return {
        motto: 'Hello miniprograme',
        userInfo: {},
        userIsGet: true
      }
    },
    created () {
      this.initUserInfo()
    },
    methods: {
      initUserInfo () {
        // 获取用户信息
        wx.getSetting({
          success: res => {
            if (res.authSetting['scope.userInfo']) {
              console.log('getSetting', res.authSetting['scope.userInfo'])
              // 已经授权，可以直接调用 getUserInfo 获取头像昵称，不会弹框
              wx.getUserInfo({
                success: res => {
                  console.log(res.userInfo)
                  this.userInfo = res.userInfo
                  this.userIsGet = true
                }
              })
            } else {
              this.userIsGet = false
            }
          }
        })
      }
    },
    components: {
      card
    }
  }
</script>

<style scoped>
  .flex {
    display: flex;
  }

  .xc {
    justify-content: center;
  }

  .yc {
    align-items: center;
  }

  .userLogo {
    margin-top: 15px;
  }

  .userLogo div {
    width: 100%;
    display: flex;
    justify-content: center;
  }

  .userLogo image {
    border-radius: 100%;
    max-height: 80px;
    max-width: 80px;
  }
</style>
