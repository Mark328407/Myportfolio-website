<template>
  <section id="contact" class="contact-section">
    <div class="container">
      <h2 class="text-center contact-title">Contact</h2>
      <div class="row g-4 align-items-center">

        <!-- Map -->
        <div class="col-lg-6">
          <div class="map-container">
            <iframe src="https://www.google.com/maps?q=Manila,Philippines&output=embed"></iframe>
          </div>
        </div>

        <!-- Contact Form -->
        <div class="col-lg-6">
          <div class="contact-form">

            <input
              type="text"
              class="form-control"
              placeholder="First Name MI Last Name"
              v-model="name"
              required
            />

            <input
              type="email"
              class="form-control"
              placeholder="Email"
              v-model="email"
              required
            />

            <textarea
              rows="5"
              class="form-control"
              placeholder="Message"
              v-model="message"
              required
            ></textarea>

            <!-- reCAPTCHA Widget -->
            <div ref="recaptchaContainer" class="mb-3"></div>

            <div class="d-flex justify-content-between align-items-center mt-3">
              <div class="social-icons">
                <a href="#"><i class="bi bi-linkedin"></i></a>
                <a href="#"><i class="bi bi-github"></i></a>
                <a href="#"><i class="bi bi-envelope-fill"></i></a>
              </div>

              <button
                class="btn btn-submit"
                @click="submitForm"
                :disabled="isLoading"
              >
                {{ isLoading ? 'Sending...' : 'Submit' }}
              </button>
            </div>

          </div>
        </div>

      </div>
    </div>
  </section>
</template>

<script setup>
  import { ref, onMounted, onBeforeUnmount } from 'vue';
  import { Notyf } from 'notyf';
  import 'notyf/notyf.min.css';

  const notyf = new Notyf();

  const WEB3FORMS_ACCESS_KEY = "1e3de487-966a-4f57-9722-58cdfbc442d0";
  const subject = "New message from Portfolio Contact Form";

  const name = ref("");
  const email = ref("");
  const message = ref("");
  const isLoading = ref(false);

  const submitForm = async () => {
  
    if (!recaptchaToken.value) {
      notyf.error('Please verify that you are not a robot.');
      return;
    }

    isLoading.value = true;

    try {
      const response = await fetch("https://api.web3forms.com/submit", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          access_key: WEB3FORMS_ACCESS_KEY,
          subject: subject,
          name: name.value,
          email: email.value,
          message: message.value
        })
      });

      const result = await response.json();

      if (result.success) {
        isLoading.value = false;
        notyf.success("Message Sent!");
      
        name.value = "";
        email.value = "";
        message.value = "";
        resetRecaptcha();
      }
    } catch (e) {
      console.log(e);
      isLoading.value = false;
      notyf.error("Failed to send a message. Please try again.");
      resetRecaptcha();
    }
  };

  const SITE_KEY = '6Lfle8QsAAAAAGPyLjHcZ_aos8BqZYElh6X8qaZ1';

  const recaptchaContainer = ref(null);
  const recaptchaWidgetId = ref(null);
  const recaptchaToken = ref('');

  function onRecaptchaSuccess(token) {
    recaptchaToken.value = token;
  }

  function onRecaptchaExpired() {
    recaptchaToken.value = '';
  }

  function renderRecaptcha() {
    if (!window.grecaptcha) {
      console.error('reCAPTCHA not loaded');
      return;
    }

    recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
      sitekey: SITE_KEY,
      size: 'normal',
      callback: onRecaptchaSuccess,
      'expired-callback': onRecaptchaExpired,
    });
  }

  function resetRecaptcha() {
    if (recaptchaWidgetId.value !== null) {
      window.grecaptcha.reset(recaptchaWidgetId.value);
      recaptchaToken.value = '';
    }
  }


  let intervalId = null;

  onMounted(() => {
    intervalId = setInterval(() => {
      if (window.grecaptcha && window.grecaptcha.render) {
        renderRecaptcha();
        clearInterval(intervalId);
        intervalId = null;
      }
    }, 100);
  });


  onBeforeUnmount(() => {
    if (intervalId !== null) {
      clearInterval(intervalId);
    }
  });
</script>