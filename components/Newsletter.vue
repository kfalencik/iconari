<template>
  <div class="newsletter">
    <div class="section">
      <div class="container">
        <div><b-icon icon="email-outline" custom-size="mdi-36px"></b-icon></div>
        <h3>Join our newsletter</h3>
        <p>Sign up to our newsletter and get <strong>5% off</strong> your next order. Pretty sweet, we know!</p>

        <mailchimp-subscribe
          :url="mailChimpApi"
          :user-id="mailChimpUid"
          :list-id="mailChimpList"
          @success="onSuccess"
        >

        <template v-slot="{ subscribe, setEmail, error }">
          <form @submit.prevent="subscribe">
            <div :class="{'messages': true, 'error': error}">
              <div v-if="error" v-html="error"></div>
            </div>
              <b-field position="is-centered">
                <b-input
                  placeholder="Your email address"
                  icon="mail"
                  id="newsletter-email"
                  type="email"
                  v-model="email" @input="setEmail(email)"
                >
                </b-input>
                <button class="button button--secondary" type="submit">Subscribe</button>
            </b-field>             
          </form>
        </template>
        </mailchimp-subscribe>
      </div>
    </div>
  </div>
</template>

<script>
import MailchimpSubscribe from 'vue-mailchimp-subscribe'

export default {
  data() {
    return {
      email: '',
      mailChimpApi: process.env.MAILCHIMP_API,
      mailChimpUid: process.env.MAILCHIMP_UID,
      mailChimpList: process.env.MAILCHIMP_LIST
    }
  },
  components: {
    MailchimpSubscribe,
  },
  methods: {
    onSuccess(event) {
      this.$store.commit('addNewsletter', this.email);
    }
  }
}
</script>

<style lang="scss" scoped>
  .newsletter {
    margin-top: 50px;
    background: $black;
    color: $white;

    h3 {
      margin-top: 0;
    }

    .container {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
    }

    .messages.error {
      background: #ff3860;
      color: #fff;
      text-align: center;
      padding: 15px;
      margin-bottom: 25px;
      max-width: 100%;  
      border-radius: 50px;

      a {
        color: #fff;
        text-decoration: underline;
      }
    }
  }
</style>
