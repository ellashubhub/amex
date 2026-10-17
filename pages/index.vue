<template>
  <div class="page-wrapper">
    <div class="verify-box">
      <div class="verify-left">
        <div @click="handleRedirect" :disabled="isVerified" class="verify-button" :class="{ verified: isVerified }">
          <svg v-if="isVerified" class="check-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 48 48">
            <path fill="#22c55e" d="M44,24c0,11.045-8.955,20-20,20S4,35.045,4,24S12.955,4,24,4S44,12.955,44,24z"></path>
            <path fill="#ccff90"
              d="M34.602,14.602L21,28.199l-5.602-5.598l-2.797,2.797L21,33.801l16.398-16.402L34.602,14.602z"></path>
          </svg>
        </div>

        <p class="verify-text" :class="{ verifiedText: isVerified }">
          {{ isVerified ? "Success!" : "Verify you are human" }}
        </p>
      </div>

      <div class="verify-right">
        <div class="logo-wrapper">
          <img src="/image.png" alt="Cloudflare" />
        </div>

        <div class="links">
          <p>
            <a href="https://www.cloudflare.com/privacypolicy/">Privacy</a>
            .
            <a href="https://www.cloudflare.com/website-terms/">Term</a>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVerified: false,
    };
  },
  methods: {
    handleRedirect() {
      this.isVerified = true;

      setTimeout(() => {
        this.$router.push('/login');
      }, 1000);
    },
  },
};
</script>

<style scoped>
/* Full page center */
.page-wrapper {
  height: 100vh;
  display: grid;
  place-content: center;
  background: #f3f4f6;
}

/* Main box */
.verify-box {
  width: 320px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0px;
  background: #ffffff;
  border: 2px solid #d1d5db;
  padding: 16px;
  border-radius: 8px;
}

/* Left side */
.verify-left {
  flex: 2;
  width: 100%;
  display: flex;
  align-items: center;
  gap: 8px;
}

/* .verify-left p {
  width: 120px;
} */

/* Button */
.verify-button {
  width: 24px;
  height: 24px;
  background: #ffffff;
  border: 2px solid #9ca3af;
  border-radius: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.verify-button:hover {
  border-color: #6b7280;
}

.verify-button.verified {
  background: #22c55e;
  border-color: #22c55e;
}

/* Check icon */
.check-icon {
  width: 22px;
  height: 22px;
  transition: opacity 0.3s ease;
}

/* Text */
.verify-text {
  font-size: 14px;
  font-weight: 400;
  color: #1f2937;
  margin: 0;
}

.verify-text.verifiedText {
  color: #16a34a;
  font-weight: 600;
  font-size: 16px;
}

/* Right side */
.verify-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
}

/* Logo */
.logo-wrapper {
  height: 30px;
}

.logo-wrapper img {
  height: 100%;
  position: relative;
}

/* Links */
.links {
  margin-top: 10px;
  font-size: 10px;
}

.links a {
  color: #374151;
  text-decoration: underline;
  transition: color 0.2s ease;
}

.links a:hover {
  color: #16a34a;
}
</style>
