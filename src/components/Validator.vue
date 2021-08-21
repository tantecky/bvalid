<template>
  <div class="container">
    <h1>
      <a href="https://en.bitcoin.it/wiki/Invoice_address" target="_blank"
        >BTC Address</a
      >
      Validator
    </h1>
    <input
      type="text"
      autocomplete="off"
      spellcheck="false"
      placeholder="Paste Here"
      v-model="inputAddr"
      @input="check"
      maxlength="42"
      :class="[{ valid: isValid }, { invalid: isInvalid }]"
    />
    <div class="note">It never leaves your browser</div>
    <div
      v-if="this.dirty"
      class="result"
      :class="[{ 'valid-text': isValid }, { 'invalid-text': isInvalid }]"
    >
      {{ this.msg }}
    </div>
    <div class="footer">
      <a href="https://github.com/tantecky/bvalid" target="_blank">github</a>
    </div>
  </div>
</template>

<script>
import { getAddressInfo } from 'bitcoin-address-validation'

function identifyFormat(type) {
  switch (type) {
    case 'p2pkh':
      return 'P2PKH'
    case 'p2sh':
      return 'P2SH'
    case 'p2wpkh':
    case 'p2wsh':
      return 'Bech32'
    default:
      return 'unknown'
  }
}

export default {
  name: 'Validator',
  data() {
    return {
      inputAddr: '',
      valid: false,
      dirty: false,
      msg: '',
    }
  },
  computed: {
    isValid() {
      return this.dirty && this.valid
    },
    isInvalid() {
      return this.dirty && !this.valid
    },
  },
  methods: {
    check() {
      this.inputAddr = this.inputAddr.replace(/ /g, '')

      if (!this.inputAddr) {
        this.dirty = false
        return
      }

      try {
        const info = getAddressInfo(this.inputAddr)
        console.log(info)
        this.valid = true

        if (info.network != 'mainnet') {
          this.valid = false
          this.msg = `Invalid network - ${info.network}`
        } else {
          this.msg = `Valid address - ${identifyFormat(info.type)}`
        }
      } catch {
        this.valid = false
        this.msg = 'Invalid address'
      } finally {
        this.dirty = true
      }
    },
  },
}
</script>

<style>
a {
  color: #0d6efd;
  text-decoration: underline;
}

h1 {
  font-size: 36px;
  margin-bottom: 8px;
}

.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  height: 100vh;
  width: 100%;
}

input {
  font-size: 32px;
  max-width: 750px;
  width: 100%;
  text-align: center;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

input:focus {
  color: #495057;
  background-color: #fff;
  border-color: #80bdff;
  outline: 0;
  box-shadow: 0 0 0 0.2rem rgb(0 123 255 / 25%);
}

.note {
  margin-top: 4px;
  font-size: 14px;
  color: #ccc;
}

.result {
  font-size: 32px;
  position: absolute;
  font-weight: bold;
  margin-top: 180px;
}

.valid {
  color: #28a745 !important;
  border-color: #28a745 !important;
  box-shadow: 0 0 0 0.2rem rgb(40 167 69/ 25%) !important;
}

.valid-text {
  color: #28a745 !important;
}

.invalid {
  color: #dc3545 !important;
  border-color: #dc3545 !important;
  box-shadow: 0 0 0 0.2rem rgb(220 53 69/ 25%) !important;
}

.invalid-text {
  color: #dc3545 !important;
}
.footer {
  position: absolute;
  bottom: 40px;
  color: #ccc;
}

@media only screen and (max-width: 576px) {
  h1 {
    font-size: 26px;
  }

  input {
    font-size: 16px;
  }

  .result {
    font-size: 22px;
  }
}
</style>
