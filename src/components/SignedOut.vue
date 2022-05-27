<template>
  <div class="form-signout">
    <img src="https://s3-us-west-1.amazonaws.com/compliance-ico-af-us-west-1/production/token_profiles/logos/original/9d5/c43/cc-/9d5c43cc-e232-4267-aa8a-8c654a55db2d-1608222929-b90bbe4696613e2faeb17d48ac3aa7ba6a83674a.png" alt="">
    <span style="color:black;">Hello World</span>
    <img src="https://images.squarespace-cdn.com/content/5b33c1c8620b8581cba04f0f/1530120246289-BASP0F0S5MBELBUHCOA2/93246a_917907056a344f42b24bf8a2524936a0_mv2.gif?content-type=image%2Fgif" alt="">
    <p style="text-align: center; margin-top: 2.5em">
      <button v-on:click="login">Sign in</button>
    </p>
  </div>
</template>

<script>
import { login } from "../utils";

export default {
  name: "SignedOut",

  methods: {
    login() {
      console.log("calling utils.login");
      login();
    },
  },
};
</script>
<style scoped>
.form-signout{
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 30px !important;
  width: 400px;
  height: 500px;
  padding: 30px;
  background-color: #fff;
  margin: auto;
  box-shadow: 1px 2px 8px rgba(0, 0, 0, 0.2);
  border-radius: 8px;
}
img{
  width: 150px;
}
span{
  margin-top: 20px;
  font-size: 40px;
  font-weight: 900;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
button{
  padding:10px 50px;
  background-color: black;
  color: white;
  border-radius: 10px;
}
</style>