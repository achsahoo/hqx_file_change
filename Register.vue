<template>
  <Head title="Register" />

  <jet-authentication-card>
    <template #logo>
      <jet-authentication-card-logo />
    </template>

    <jet-validation-errors class="mb-4" />

    <form @submit.prevent="submit">
      <div>
        <jet-label
          for="username"
          value="Enter your TCS NextStep reference CT/DT ID (Will be used as Username) *"
        />
        <span class="text-gray-400 text-sm">
          Don't have a TCS NextStep reference CT/DT ID? Login to
          <a
            href="https://nextstep.tcs.com/"
            class="underline text-sm text-gray-300 hover:text-gray-900"
            >TCS NextStep</a
          >
          to create one and come back here to register with that TCS NextStep
          reference CT/DT ID.
        </span>
        <jet-input-with-validation
          :showTick="current > 0"
          :isValid="isUsernameValid"
          id="username"
          type="text"
          class="mt-1 block w-full"
          v-model="form.username"
          required
          autofocus
          pattern="[CD]T20[12][0-9]{8}"
          @input="changeCurr(1)"
          @blurred="changeCurr(1)"
          invalidMsg="TCS NextStep reference CT/DT ID is not valid"
        />
      </div>
      <div class="mt-4">
        <jet-label for="name" value="Name *" />
        <jet-input-with-validation
          :showTick="current > 1"
          :isValid="isNameValid"
          id="name"
          type="text"
          class="mt-1 block w-full"
          v-model="form.name"
          required
          autofocus
          autocomplete="name"
          @focused="changeCurr(2)"
          @blurred="changeCurr(2)"
          invalidMsg="Name is not valid"
        />
      </div>

      <div class="mt-4">
        <jet-label for="email" value="Email *" />
        <jet-input-with-validation
          :showTick="current > 2"
          :isValid="isEmailValid"
          id="email"
          type="email"
          class="mt-1 block w-full"
          v-model="form.email"
          required
          @focused="changeCurr(3)"
          @blurred="changeCurr(3)"
          invalidMsg="Email is not valid"
        />
      </div>

      <div class="mt-4">
        <jet-label for="college" value="College *" />
        <select id="college" v-model="form.college" class="mt-1 block w-full">
          <option value="">Select your college</option>
          <option value="clg1">College 1</option>
          <option value="clg2">College 2</option>
          <option value="other">Other</option>
        </select>
        <div v-if="form.college === 'other'" class="mt-2">
          <jet-label for="college_other" value="Enter your college name" />
          <jet-input
            id="college_other"
            type="text"
            class="mt-1 block w-full"
            v-model="form.college_other"
          />
        </div>
      </div>

      <div class="mt-4">
        <jet-label for="password" value="Password *" />
        <jet-input-with-validation
          :showTick="current > 3"
          :isValid="isPasswordValid"
          id="password"
          type="password"
          class="mt-1 block w-full"
          v-model="form.password"
          required
          autocomplete="new-password"
          @focused="changeCurr(4)"
          @blurred="changeCurr(4)"
          invalidMsg="Password is not Compliant. It must contain at least 8 characters with at least one lowercase alphabet, one uppercase alphabet, one numeric digit and one symbol from #?!@$%^&*-"
        />
      </div>

      <div class="mt-4">
        <jet-label for="password_confirmation" value="Confirm Password *" />
        <jet-input-with-validation
          :showTick="current > 4"
          :isValid="doPasswordsMatch"
          id="password_confirmation"
          type="password"
          class="mt-1 block w-full"
          v-model="form.password_confirmation"
          required
          autocomplete="new-password"
          @focused="changeCurr(5)"
          @blurred="changeCurr(5)"
          invalidMsg="Passwords do not match"
        />
      </div>

      <div class="mt-4">
        <jet-label for="terms">
          <div class="flex items-center">
            <jet-checkbox
              name="terms"
              id="terms"
              v-model:checked="form.terms"
            />

            <div class="ml-2">
              I agree to the
              <a
                target="_blank"
                href="https://www.tcs.com/privacy-policy"
                class="underline text-sm text-gray-300 hover:text-gray-900"
                >TCS Privacy Policy</a
              >,
              <a
                target="_blank"
                href="/cookie-policy"
                class="underline text-sm text-gray-300 hover:text-gray-900"
                >Cookie Policy</a
              >
              and
              <a
                target="_blank"
                :href="route('terms.show')"
                class="underline text-sm text-gray-300 hover:text-gray-900"
                >Terms &amp; Conditions</a
              >.
            </div>
          </div>
        </jet-label>
      </div>
      <div class="mt-4 flex items-center">
        <jet-label for="year">
          <div class="flex items-center">
            <jet-checkbox name="year" id="year" v-model:checked="form.year" />
            <div class="ml-2">
              I am a student from the 2025 batch of
              B.Tech/M.Tech/BCA/MCA/BSc/MSc from an accredited institute in
              India.
            </div>
          </div>
        </jet-label>
      </div>

      <div class="flex items-center justify-end mt-4">
        <Link
          :href="route('login')"
          class="underline text-sm text-gray-300 hover:text-gray-900"
          >Already registered?
        </Link>

        <jet-button
          class="ml-4"
          :class="{
            'opacity-25 cursor-not-allowed': form.processing || !isFormValid,
          }"
          :disabled="form.processing || !isFormValid"
          >Register</jet-button
        >
      </div>
    </form>
  </jet-authentication-card>
</template>

<script>
import { defineComponent } from "vue";
import JetAuthenticationCard from "@/Jetstream/AuthenticationCard.vue";
import JetAuthenticationCardLogo from "@/Jetstream/AuthenticationCardLogo.vue";
import JetButton from "@/Jetstream/Button.vue";
import JetInput from "@/Jetstream/Input.vue";
import JetInputWithValidation from "@/Jetstream/InputWithValidation.vue";
import JetCheckbox from "@/Jetstream/Checkbox.vue";
import JetLabel from "@/Jetstream/Label.vue";
import JetValidationErrors from "@/Jetstream/ValidationErrors.vue";
import { Head, Link } from "@inertiajs/vue3";

export default defineComponent({
  components: {
    Head,
    JetAuthenticationCard,
    JetAuthenticationCardLogo,
    JetButton,
    JetInput,
    JetInputWithValidation,
    JetCheckbox,
    JetLabel,
    JetValidationErrors,
    Link,
  },

  data() {
    return {
      form: this.$inertia.form({
        name: "",
        email: "",
        username: "",
        password: "",
        password_confirmation: "",
        terms: false,
        year: false,
        token: "",
        college: "",
        college_other: "",
      }),
      current: 0,
    };
  },

  methods: {
    submit() {
      // e.preventDefault();
      // grecaptcha.ready(() => {
      //     grecaptcha.execute("6Ldhh_cdAAAAAPtt0ADe1bqjRDyAwcB14GnDaf01", { action: "submit" }).then((token) => {
      // Add your logic to submit to your backend server here.
      // this.form.token = token;
      this.form.post(this.route("register"), {
        onFinish: () => this.form.reset("password", "password_confirmation"),
      });
      //     });
      // });
    },
    changeCurr(i) {
      this.current = Math.max(this.current, i);
    },
  },
  mounted() {
    this.current = 0;
  },
  computed: {
    isUsernameValid() {
      return /^[CD]T20[12][0-9]{8}$/.test(this.form.username);
    },
    isNameValid() {
      return /^[A-Za-z ]+$/.test(this.form.name) && this.form.name.length > 2;
    },
    isEmailValid() {
      return /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(
        this.form.email
      );
    },
    isPasswordValid() {
      return (
        /[A-Z]/.test(this.form.password) &&
        /[a-z]/.test(this.form.password) &&
        /[0-9]/.test(this.form.password) &&
        /[#?!@$%^&*-]/.test(this.form.password) &&
        this.form.password.length > 7
      );
    },
    doPasswordsMatch() {
      return this.form.password == this.form.password_confirmation;
    },
    isFormValid() {
      return (
        this.isUsernameValid &&
        this.isNameValid &&
        this.isEmailValid &&
        this.isPasswordValid &&
        this.doPasswordsMatch
      );
    },
  },
});
</script>
