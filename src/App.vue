<template>
  <div id="app">
    <h1>Phone validation example</h1>

    <div>
      <a href="https://amliu.github.io/libphonenumber-example/" target="_blank">
        <img :src="gitIcon" alt="Github icon" width="48">
      </a>
    </div>

    <p>
      Use JS lib
      <a href="https://github.com/ruimarinho/google-libphonenumber" target="_blank">
        ruimarinho/google-libphonenumber
      </a>
      to validate international mobile numbers.
    </p>

    <p>
      The "FORMAT VALID" button will be enabled only when the phone number is valid. Otherwise, disabled.
    </p>

    <md-field>
      <label for="countries">Country</label>
      <md-select v-model="countryISO" name="countries" id="countries">
        <md-option
          v-for="c in countries"
          :key="c.region_code"
          :value="c.region_code"
        >
          {{ c.label }}
        </md-option>
      </md-select>
    </md-field>

    <md-field>
      <label for="phone">Phone number</label>
      <md-input
        id="phone"
        v-model.lazy="phone"
        placeholder="Input mobile phone number"
        type="tel"
      />
    </md-field>

    <md-button
      class="md-raised md-primary"
      :disabled="!phoneValid"
    >
      FORMAT VALID
    </md-button>

    <section
      id="format-section"
      v-if="phoneValid"
    >
      <h2>Phone number in different formats</h2>
      <div>E164 : {{ formatE164 }}</div>
      <div>INTERNATIONAL : {{ formatInternational }} </div>
      <div>NATIONAL : {{ formatNational }} </div>
      <div>RFC3966 : {{ formatRFC3966 }} </div>
    </section>
  </div>
</template>

<script>
import countries from './region_code';
import * as phoneValidator from './phoneValidator';
import GitIcon from './assets/GitHub-Mark-64px.png';

export default {
  name: 'app',
  data () {
    return {
      phone: '',
      countries,
      countryISO: 'TW',
      gitIcon: GitIcon,
    }
  },
  computed: {
    phoneValid: function () {
      if (!this.phone.length) {
        return false;
      }
      return  phoneValidator.isPhoneNumberValid(this.phone, this.countryISO);
    },

    formatE164: function () {
      return phoneValidator.formatE164(this.phone, this.countryISO);
    },

    formatInternational: function () {
      return phoneValidator.formatInternational(this.phone, this.countryISO);
    },

    formatNational: function () {
      return phoneValidator.formatNational(this.phone, this.countryISO);
    },

    formatRFC3966: function () {
      return phoneValidator.formatRFC3966(this.phone, this.countryISO);
    },
  },
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 10% auto;
  width: 90%;
  max-width: 500px;
}

#format-section {
  > div {
    margin-bottom: 1rem;
    font-size: 1.2rem;
    color: cadetblue;
  }
}

html, body {
  width: 100%;
  height: 100%;
  font-size: 16px;
}

</style>
