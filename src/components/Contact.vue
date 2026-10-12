<script setup>
    import { ref, onMounted, onBeforeMount } from 'vue';
    import { Notyf } from 'notyf';
    import 'notyf/notyf.min.css';

    const notyf = new Notyf();

    const WEB3FORMS_ACCESS_KEY = "83729160-1abc-4ffc-bc57-8159a793e1fe";

    const subject = "New message from Portfolio Contact Form";

    const name = ref("");
    const email = ref("");
    const message = ref("");
    const isLoading = ref(false);

    const submitForm = async () => {

    	if(!recaptchaToken.value) {
            notyf.error('Please verify that you are not a robot');
            return;
        }

        isLoading.value = true;

        try {
            const response = await fetch("https://api.web3forms.com/submit", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Accept: "application/json"
                },
                body: JSON.stringify({
                    access_key: WEB3FORMS_ACCESS_KEY,
                    subject: subject,
                    email: email.value,
                    name: name.value,
                    message: message.value
                })
            })

            const result = await response.json();

            if(result.success) {
                console.log(result);
                isLoading.value = false;
                notyf.success("Message Sent!")
            }
        } catch(e) {
            console.log(e);
            isLoading.value = false;
            notyf.error("Failed to send message. Please try again.")
        } 
        finally {
            resetRecaptcha();
        }
    } 

   const SITE_KEY = '6LfSp-stAAAAAEKVHVCYnKpU4xPbu_GmcHjKdsCT';  // Replace with your site key

const recaptchaContainer = ref(null);
const recaptchaWidgetId = ref(null);
const recaptchaToken = ref('');

// Callback called by reCAPTCHA when successful
function onRecaptchaSuccess(token) {
  recaptchaToken.value = token;
}

// Callback when expired
function onRecaptchaExpired() {
  recaptchaToken.value = '';
}

// Function to render the reCAPTCHA widget
function renderRecaptcha() {
  if (!window.grecaptcha) {
    console.error('reCAPTCHA not loaded');
    return;
  }

  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
    sitekey: SITE_KEY,
    size: 'normal', // or 'compact'
    callback: onRecaptchaSuccess,
    'expired-callback': onRecaptchaExpired,
  });
}

// Function to reset reCAPTCHA 
function resetRecaptcha() {
  if (recaptchaWidgetId.value !== null) {
    window.grecaptcha.reset(recaptchaWidgetId.value);
    recaptchaToken.value = '';
  }
}

onMounted(() => {
  // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
  // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
  // Callback functions handle success and expiration events. 
  // reCAPTCHA is reset upon form submission to clear the token.
  const interval = setInterval(() => {
    if (window.grecaptcha && window.grecaptcha.render) {
      renderRecaptcha();
      clearInterval(interval);
    }
  }, 100);

  onBeforeUnmount(() => {
    clearInterval(interval);
  });
});


</script>



<template>
	<!--Contact Section Start-->
<Section id="contact" class="container text-center py-3 my-3" >
	<div class="justify-content-center align-items-center p-3">
	<h2>Contact Us</h2>

	<div class="row justify-content-center align-items-stretch">

	        <!-- Map Column (Left Side) Start-->
	        <div class="col-lg-6 mb-4 mb-lg-0 mx-auto">
	            <div class="h-100 rounded overflow-hidden" style="min-height: 400px; border: 1px solid #334155;">
	                <iframe 
	                    src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3864.375545059364!2d120.97781900000001!3d14.405501800000012!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3397d22d15880463%3A0x75bb8974052f85e3!2sJollibee%20Molino!5e0!3m2!1sen!2sph!4v1784197897529!5m2!1sen!2sph"
	                    width="100%" 
	                    height="100%" 
	                    style="border:0; min-height: 400px;" 
	                    allowfullscreen="" 
	                    loading="lazy" 
	                    referrerpolicy="no-referrer-when-downgrade">
	                </iframe>
	            </div>
	        </div>
 			<!-- Map Column (Left Side) End-->

 			<!-- Form Column (Right Side) Start-->
		<div class="col-md-4 mt-md-3 mx-auto" id="form-col">

			<form @submit.prevent="submitForm" class="p-3 rounded">

				<div class="mb-3">
     				<label for="exampleInputEmail1" class="form-label" >Name</label>
    				<input type="text" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" v-model="name">

				</div>
     				<label for="exampleInputEmail1" class="form-label">Email address</label>
    				<input type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" v-model="email">	

  					<div class="mb-3">
  						<label for="exampleInputPassword1" class="form-label">Message</label>
  						<textarea class="form-control" rows="7" id="Message" v-model="message"></textarea>

  					<!-- recaptcha checkbox -->
                  <div class="d-flex justify-content-end mt-2">
                        <div ref="recaptchaContainer"></div>
                    </div>

  					</div>
  				<button type="submit" class="btn btn-primary" :disabled="isLoading">{{ isLoading ? "Sending..." : "Submit" }}</button>	
  				</form>
  			</div>
		</div>	
</div>
</Section>
</template>