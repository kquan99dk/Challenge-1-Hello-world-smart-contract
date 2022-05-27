<template>
  <div>
    <div style="float: right">
      <div style="display: flex; align-items: center;">
        <img style="width: 30px; margin-right: 5px;" src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/7c/User_font_awesome.svg/2048px-User_font_awesome.svg.png" alt="">
        <span style="margin-right: 15px;">{{ accountId }}</span>
        <button class="btn-signout" v-on:click="logout">Sign out</button>
      </div>
    </div>
    
    <main>
      <div class="logoNear">
        <img class="logoNear-img" src="https://s3-us-west-1.amazonaws.com/compliance-ico-af-us-west-1/production/token_profiles/logos/original/9d5/c43/cc-/9d5c43cc-e232-4267-aa8a-8c654a55db2d-1608222929-b90bbe4696613e2faeb17d48ac3aa7ba6a83674a.png" alt="">
      </div>
      <h3 style="text-align: center;">Welcome to the Near App</h3>
      <div style="margin-top: 30px">
        <p style="margin-bottom: 0;text-align: center;font-size: 50px">Hello <b>{{ savedGreeting }}</b></p>
      </div>
      <form v-on:submit.prevent="saveGreeting">
        <fieldset ref="fieldset">
          <label
            for="greeting"
            style="display:block; color:black;margin-bottom:0.5em;"
          >Make a greeting</label>
          <!-- <div style="display:flex">
            <input v-model="newGreeting" autocomplete="off" id="greeting" style="flex:1" />
            <button id="save" style="border-radius:0 5px 5px 0">Save</button>
          </div> -->
          <div style="display: flex; flex-direction: column; align-items: center">
            <input v-model="newGreeting" placeholder="Type your name" autocomplete="off" id="greeting" style="width: 100%; border-radius: 8px;height: 50px;" />
            <button style="border-radius:15px; margin-top: 10px; padding:10px 50px;background:rgb(174, 136, 11);">Save</button>
          </div>
        </fieldset>
      </form>
    </main>

    <Notification
      v-show="notificationVisible"
      ref="notification"
      :networkId="networkId"
      :msg="'User App Near Hello World'"
      :contractId="contractId"
      :visible="false"
    />
  </div>
</template>

<script>
import { logout } from "../utils"

import Notification from "./Notification.vue"

export default {
  name: "SignedIn",

  beforeMount() {
    if (this.isSignedIn) {
      this.retrieveSavedGreeting()
    }
  },

  components: {
    Notification,
  },

  data: function () {
    return {
      savedGreeting: "",
      newGreeting: "",
      notificationVisible: false,
    }
  },
  created() {
    // eslint-disable-next-line no-undef
    // console.log('kkkkkkkkkkkk', process.env.VUE_APP_CONTRACT_NAME);
  },
  computed: {
    isSignedIn() {
      return window.walletConnection? window.walletConnection.isSignedIn(): false
    },
    accountId() {
      return window.accountId
    },
    contractId() {
      return window.contract? window.contract.contractId: null
    },
    networkId() {
      return window.networkId
    },
  },

  methods: {
    retrieveSavedGreeting() {
      //retrieve greeting
      window.contract
        .get_greeting({ account_id: window.accountId })
        .then((greetingFromContract) => {
          console.log('retrieveSavedGreeting', greetingFromContract);
          this.savedGreeting = ''
          this.newGreeting = ''
        })
    },

    saveGreeting: async function () {
      // fired on form submit button used to update the greeting

      // disable the form while the value gets updated on-chain
      this.$refs.fieldset.disabled = true

      try {
        
        // make an update call to the smart contract
        await window.contract.set_greeting({
          // pass the new greeting
          message: this.newGreeting,
        })
      } catch (e) {
        alert(
          "Something went wrong! " +
            "Maybe you need to sign out and back in? " +
            "Check your browser console for more info."
        )
        throw e //re-throw
      } finally {
        // re-enable the form, whether the call succeeded or failed
        this.$refs.fieldset.disabled = false
      }

      // update savedGreeting with persisted value
      this.savedGreeting = this.newGreeting

      this.notificationVisible = true //show new notification

      // remove Notification again after css animation completes
      // this allows it to be shown again next time the form is submitted
      setTimeout(() => {
        this.notificationVisible = false
      }, 11000)

    },

    logout: logout,
  },
}
</script>
<style scoped>
.btn-signout{
  padding:10px 50px;
  background-color: rgb(0, 0, 0);
  color: white;
  border-radius: 10px;
}
.logoNear{
  display: flex;
  justify-content: center;
}
.logoNear-img{
  width: 150px;
  margin: auto;
}
</style>