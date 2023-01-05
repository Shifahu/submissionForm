<template>
  <form id="signup-form" v-on:submit.prevent="submit">
    <div class="row">
      <div class="col-12 form-group">
        <label class="header">
          Title
          <span class="text-danger">*</span>
        </label>
        <select
          v-model.trim="$v.title.$model"
          :class="{'is-invalid': validationStatus($v.title)}"
          class="form-control form-control-lg"
        >
          <option value>Select title</option>
          <option value="Mr">Mr</option>
          <option value="Mrs">Mrs</option>
          <option value="Ms">Ms</option>
        </select>
        <div v-if="!$v.title.required" class="invalid-feedback">The title field is required.</div>
      </div>

      <div class="col-12 form-group">
        <label class="header">
          Full Name
          <span class="text-danger">*</span>
        </label>
        <input
          type="text"
          v-model.trim="$v.fullname.$model"
          :class="{'is-invalid': validationStatus($v.fullname)}"
          class="form-control form-control-lg"
        />
        <div v-if="!$v.fullname.required" class="invalid-feedback">The full name field is required.</div>
      </div>
      <div class="calendar">
        <label for="start-date" style="display: flex">Policy Start Date:</label>
        <input type="date" v-model="date" id="start-date" name="start-date" />
      </div>
      <hr size="8" width="90%" color="red" />

      <div class="col-12 form-group">
        <label class="header">
          Employee ERN
          <span class="text-danger">*</span>
        </label>
        <input
          type="text"
          v-model.trim="$v.employeeErn.$model"
          :class="{'is-invalid': validationStatus($v.employeeErn) && status===false}"
          class="form-control form-control-lg"
        />
        <div v-if="status===false" class="invalid-feedback">Employee ERN required</div>
      </div>

      <div class="checkbox">
        <input type="checkbox" id="ERN-exempt" name="ERN-exempt" v-model="status" />
        <label for="exemption">We're exempt from having an ERN or can</label>
      </div>

      <hr size="8" width="90%" />
      <div class="col-12 form-group">
        <label class="header">
          Premises Address
          <span class="text-danger">*</span>
        </label>
        <textarea
          type="text"
          v-model.trim="$v.address.$model"
          :class="{'is-invalid': validationStatus($v.address)}"
          class="form-control form-control-lg"
        />
        <div v-if="!$v.address.required" class="invalid-feedback">The full address is required.</div>
      </div>
      <div class="col-12 form-group">
        <label class="header">
          Email
          <span class="text-danger">*</span>
        </label>
        <input
          type="email"
          v-model.trim="$v.email.$model"
          :class="{'is-invalid': validationStatus($v.email)}"
          class="form-control form-control-lg"
        />
        <div v-if="!$v.email.required" class="invalid-feedback">The email field is required.</div>
        <div v-if="!$v.email.email" class="invalid-feedback">The email is not valid.</div>
      </div>
      <div class="col-12 form-group">
        <label class="header">
          Password
          <span class="text-danger">*</span>
        </label>
        <input
          type="password"
          v-model.trim="$v.password.$model"
          :class="{'is-invalid': validationStatus($v.password)}"
          class="form-control form-control-lg"
        />
        <div v-if="!$v.password.required" class="invalid-feedback">The password field is required.</div>
        <div
          v-if="!$v.password.minLength"
          class="invalid-feedback"
        >You must have at least {{ $v.password.$params.minLength.min }} letters.</div>
        <div
          v-if="!$v.password.maxLength"
          class="invalid-feedback"
        >You must not have greater then {{ $v.password.$params.maxLength.min }} letters.</div>
      </div>
      <div class="col-12 form-group">
        <label class="header">
          Confirm Password
          <span class="text-danger">*</span>
        </label>
        <input
          type="password"
          v-model.trim="$v.confirmPassword.$model"
          :class="{'is-invalid': validationStatus($v.confirmPassword)}"
          class="form-control form-control-lg"
        />

        <div
          v-if="!$v.confirmPassword.sameAs"
          class="invalid-feedback"
        >The confirm password must match the password.</div>
      </div>
      <div class="col-12 form-group text-center">
        <button class="btn btn-vue btn-lg col-4" @click="isModalVisible = true">Confirm</button>
        <div
          class="pop-up"
          :class="{'open-popUp':isModalVisible}"
          id="pop-up"
          v-if="isModalVisible"
        >
          <h2>Thank you!</h2>
          <p>your details have been successfully submitted! thanks</p>
          <button type="button" @click="isModalVisible = false">OK</button>
        </div>
      </div>
    </div>
  </form>
</template>
<script>
import {
  required,
  email,
  minLength,
  maxLength,
  sameAs,
  helpers
} from "vuelidate/lib/validators";
// import axios from 'axios';

export default {
  name: "SignupForm",

  data: function() {
    return {
      isModalVisible: false,
      status: false,
      date: "",
      fullname: "",
      employeeErn: "",
      address: "",
      email: "",
      title: "",
      password: "",
      confirmPassword: "",
    };
  },
 computed: {
  employeeErnRule() {
    const isValid = value => {
      return this.status || (value !== '');
    };

    return helpers(isValid);
  },
},

  validations: {
    // employeeErn: {
    //   employeeErnRule(){}
    // },
    fullname: { required },
    employeeErn: { required },
    address: { required },
    email: { required, email },
    title: { required },
    password: { required, minLength: minLength(6), maxLength: maxLength(18) },
    confirmPassword: { required, sameAs: sameAs("password") }
  },

  methods: 
  {
    validationStatus: function(validation) {
      return typeof validation != "undefined" ? validation.$error : false;
    },

    submit: function() {
      this.$v.$touch();
      console.log("this is working1");
      if (this.$v.$pendding || this.$v.$error) return;
      console.log("this is working");
      alert("Data Submit");
    }
  }
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
hr {
  background-color: var(--bs-form-control-bg);
}

input#start-date {
  width: fit-content;
  border-radius: 6px;
  min-height: calc(1.5em + 1rem + calc(var(--bs-border-width) * 2));
  padding: 0.5rem 1rem;
  font-size: 1.25rem;
  border-radius: 0.5rem;
  border: var(--bs-border-width) solid var(--bs-border-color);
  line-height: 1.5;
  width: fit-content;
}
input#ERN-exempt {
  margin-right: 10px;
}

.vue-bg {
  background: #bce5d0;
}

.form-control {
  width: auto !important;
  appearance: revert;
}

.calendar {
  flex-direction: column;
}
.calendar,
.checkbox {
  display: flex;
  margin: 10px 10px 10px 0;
}

.header {
  display: flex;
  flex-direction: inherit;
  padding: 10px;
}
.btn-vue {
  background: #e31c79;
  color: #ffffff;
  font-weight: bold;
}
.pop-up {
  width: 400px;
  background: #ffffff;
  border-radius: 6px;
  position: absolute;
  top: 0;
  left: 50%;
  transform: translate(-50%, -50%) scale(0.1);
  text-align: center;
  padding: 0 30px 30px;
  color: #333;
  visibility: hidden;
  transition: 0.4s, top 0.4s;
}

.open-popUp {
  visibility: visible;
  top: 50%;
  transform: translate(-50%, -50%) scale(1);
}

.pop-up h2 {
  font-size: 38px;
  font-weight: 500;
  margin: 30px 0 10px;
}

.pop-up button {
  width: 100%;
  margin-top: 50px;
  padding: 10px 0;
  background: #6fd649;
  color: #ffffff;
  border: 0;
  outline: none;
  font-size: 18px;
  border-radius: 4px;
  cursor: pointer;
  box-shadow: 0 5px 5px rgba(0, 0, 0, 0.2);
}

.text-center {
  padding-top: 10px;
}
</style>