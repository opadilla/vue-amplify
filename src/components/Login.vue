<template>
  <div class="hello">
    <amplify-authenticator v-if="!signedIn"></amplify-authenticator>

    <amplify-sign-out v-if="signedIn"></amplify-sign-out>
  </div>
</template>

<script>
import Amplify, { Auth } from "aws-amplify";
import { AmplifyEventBus } from "aws-amplify-vue";

const awsExportsIntegrifydev = {
  aws_project_region: "us-east-1",
  aws_user_pools_id: "us-east-1_YZACgTdcm",
  aws_cognito_region: "us-east-1",

  aws_cognito_identity_pool_id: "us-east-1:552d8e00-58b5-4cb7-94ed-20ba0602b52d",
  
  
  aws_user_pools_web_client_id: "kh7od7ma26k8naj5l7p0cig2v",
  oauth: {}
};

Amplify.configure(awsExportsIntegrifydev);

export default {
  name: "Login",
  props: {
    msg: String,
    signedIn: false
  },
  created() {
    this.findUser();
    AmplifyEventBus.$on("authState", info => {
      if (info === "signedIn") this.findUser();
      else this.signedIn = false;
    });
  },
  methods: {
    async findUser() {
      try {
        const user = await Auth.currentAuthenticatedUser();
        this.signedIn = true;
        console.log(user);
      } catch (err) {
        this.signedIn = false;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
