<template>
  <div>
    <v-card class="elevation-1 my-2 pa-4">
      <div class="headline text-center primary--text">Support</div>
      <v-progress-linear
        v-if="loading"
        indeterminate
        class="my-2"
      ></v-progress-linear>
      <div v-if="!submitted && !loading">
        <div class="text-justify my-2">
          Please contact coinos with any questions you may have and we will get
          back to you as soon as possible.
        </div>
        <v-form @submit.prevent="submit">
          <v-text-field label="Email" v-model="email" />
          <v-text-field label="Account Name (optional)" v-model="account" />
          <v-textarea label="Message" v-model="message" />
          <div class="d-flex">
            <v-btn
              class="flex-grow-1 wide"
              dark
              type="submit"
              :disabled="!message || !email || submitted"
            >
              <v-icon left color="green">$question</v-icon><span>Submit</span>
            </v-btn>
          </div>
        </v-form>
      </div>
      <div v-if="submitted && !loading">
        <div class="text-justify my-2">
          Thanks for contacting us! Please check your e-mail inbox for a
          response within the next few days.
        </div>
        <div class="d-flex">
          <v-btn
            @click="$go('/home')"
            class="my-2 flex-grow-1 wide"
          >
            <v-icon>$left</v-icon>
            Back</v-btn
          >
        </div>
      </div>
    </v-card>
  </div>
</template>

<script>
import { call, get } from 'vuex-pathify';

export default {
  data: () => ({
    email: null,
    account: null,
    message: null,
    loading: false,
    submitted: false,
  }),
  methods: {
    async submit() {
      this.loading = true;
      this.submitted = await this.submitSupport({
        email: this.email,
        account: this.account,
        message: this.message,
      });
      this.loading = false;
    },
    submitSupport: call('submitSupport'),
  },
  mounted() {
    this.account = this.user ? this.user.username : null;
  },
  computed: {
    user: get('user'),
  },
};
</script>
