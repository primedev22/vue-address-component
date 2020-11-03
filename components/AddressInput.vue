<template>
  <div class="flex address-wrapper">
    <div class="tooltip">
      ?
      <span class="tooltip-text">Tooltip</span>
    </div>
    <div class="address-input" tabindex="0">
      <input
        v-model="value"
        type="text"
        placeholder="Address"
        class="address"
        @focusout="onFocusout"
      />
      <div v-show="showPredictions" class="predictions">
        <div
          v-for="(item, index) in predictions"
          :key="index"
          class="prediction"
          @click="onSelect(item)"
        >
          {{ item }}
        </div>
      </div>
      <div class="label px-1">address</div>
    </div>
    <div v-show="showNumberInput" class="number-input ml-1" tabindex="0">
      <input
        ref="numberInputRef"
        v-model="number"
        type="text"
        class="number"
        @focusout="onNumberInputFocusout"
      />
      <div class="label px-1">No</div>
    </div>
    <div>
      <div v-show="value" class="close" @click="value = ''">X</div>
      <div class="plus mt-1" @click="onPlus">+</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    placeholder: {
      type: String,
      default: '',
    },
    label: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      predictions: [],
      value: '',
      number: '',
      showPredictions: false,
      showNumberInput: false,
      lastModifiedTime: null,
    }
  },
  watch: {
    value(val) {
      if (!this.showNumberInput) {
        this.lastModifiedTime = Date.now()
        if (val === '') {
          this.predictions = []
          this.showPredictions = false
          return
        }
        this.updatePredictions(this.lastModifiedTime)
      }
    },
  },
  methods: {
    updatePredictions(modifiedTime) {
      const sessionToken = new this.$google.maps.places.AutocompleteSessionToken()
      const autocompleteService = new this.$google.maps.places.AutocompleteService()
      autocompleteService.getPlacePredictions(
        {
          input: this.value,
          type: ['address'],
          sessionToken,
        },
        (predictions, status) => {
          if (status === 'OK') {
            if (modifiedTime === this.lastModifiedTime) {
              this.predictions = predictions.map((item) => item.description)
              this.showPredictions = true
            }
          }
        }
      )
    },
    onSelect(prediction) {
      this.value = prediction
      this.showPredictions = false
      this.showNumberInput = true
      setTimeout(() => {
        this.$refs.numberInputRef.focus()
      }, 100)
    },
    onFocusout() {
      setTimeout(() => {
        this.showPredictions = false
      }, 1000)
    },
    onNumberInputFocusout() {
      if (this.number) this.value = this.number + ', ' + this.value
      this.number = ''
      setTimeout(() => {
        this.showNumberInput = false
      }, 100)
    },
    onPlus() {
      this.$emit('plus')
    },
  },
}
</script>

<style scoped>
.address-wrapper {
  width: 500px;
  height: 30px;
  margin-bottom: 20px;
}
.address-input {
  cursor: pointer;
  position: relative;
  width: 100%;
  height: 30px;
  border-bottom: 1px solid white;
  color: white;
}
.address-input:focus {
  outline: none;
}
.address {
  width: 100%;
  background: transparent;
}
.number-input {
  cursor: pointer;
  position: relative;
  width: 50px;
  height: 30px;
  border-bottom: 1px solid white;
  color: white;
}
.number {
  background: transparent;
}
.address:focus,
.number:focus {
  outline: none;
}
.label {
  position: absolute;
  font-size: 12px;
  background-color: #181818;
  top: 20px;
  right: 10px;
  text-transform: uppercase;
}
.predictions {
  position: absolute;
  left: 0;
  top: 100%;
  z-index: 1;
  width: 100%;
  margin-top: 10px;
  padding: 10px 0;
  background: #333333;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}
.prediction {
  padding: 0 10px;
}
.prediction:hover {
  opacity: 50%;
}
.close {
  cursor: pointer;
  color: white;
  width: 15px;
  height: 15px;
  font-size: 12px;
  line-height: 15px;
  border-radius: 7px;
  border: 1px solid white;
  text-align: center;
  vertical-align: middle;
}
.plus {
  cursor: pointer;
  color: white;
  font-size: 18px;
  line-height: 22px;
  text-align: center;
  vertical-align: middle;
}
.tooltip {
  cursor: pointer;
  color: white;
  width: 15px;
  height: 15px;
  font-size: 12px;
  line-height: 15px;
  border-radius: 7px;
  border: 1px solid white;
  text-align: center;
  vertical-align: middle;
  margin: 20px 5px 0 0;
}
.tooltip .tooltip-text {
  visibility: hidden;
  width: 120px;
  background-color: black;
  color: #fff;
  text-align: center;
  border-radius: 6px;
  padding: 5px 0;

  /* Position the tooltip */
  position: absolute;
  z-index: 1;
}
.tooltip:hover .tooltip-text {
  visibility: visible;
}
</style>
