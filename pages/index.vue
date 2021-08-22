<template>
  <v-main class="main-wrap">
    <v-container class="container">
      <h1 class="main-title">PX To REM calculator</h1>
      <div class="cal-content">
        <div class="input-content">
          <label class="cal-label" for="base-font">Base Font :</label>
          <input
            id="base-font"
            v-model="baseValue"
            type="text"
            class="input"
            maxlength="3"
          />PX
        </div>
        <div class="input-content">
          <label class="cal-label" for="convert">Convert Font :</label>
          <input
            id="convert"
            v-model="convertValue"
            type="text"
            class="input convert"
            placeholder="12,16,18,20"
          />
        </div>
        <div class="input-content">
          <label class="cal-label" for="decimal-point">Decimal Point :</label>
          <input
            id="decimal-point"
            v-model="decimalPoint"
            min="0"
            type="text"
            class="input"
            placeholder="12,16,18,20"
          />
        </div>
        <div class="button-wrap">
          <button class="convert-button" @click="convert">Convert</button>
        </div>
        <div v-if="convertArray.length > 0" class="convert-result">
          <h1 class="result-title">Result</h1>
          <div
            v-for="(result, index) in convertArray"
            :key="index"
            class="result"
          >
            <div class="before-convert">{{ result.key }}px</div>
            >
            <div class="after-convert">{{ result.value }}rem</div>
          </div>
        </div>
        <div v-if="convertArray.length > 0" class="button-wrap">
          <button class="convert-button" @click="makeVariables">
            Make SCSS variables
          </button>
          <p v-if="successConvert" class="scss-variables-convert-success">
            Make SCSS variables Success!!!! Check ClipBoard!
          </p>
        </div>
      </div>
    </v-container>
  </v-main>
</template>

<script>
export default {
  data() {
    return {
      baseValue: 12,
      convertValue: '',
      decimalPoint: 3,
      convertArray: [],
      successConvert: false,
    };
  },
  methods: {
    convert() {
      if (this.convertValue.length > 0) {
        this.convertArray = [];
        this.convertValue.split(',').forEach(v => {
          const convertValue = v / this.baseValue;
          if (v / this.baseValue === 1) {
            this.convertArray.push({
              key: v,
              value: convertValue,
            });
          } else {
            this.convertArray.push({
              key: v,
              value: (v / this.baseValue).toFixed(this.decimalPoint),
            });
          }
        });
      }
    },
    makeVariables() {
      let variablesText = '';
      this.convertArray.forEach((v, i) => {
        variablesText = variablesText + `$font-size-${i}: ${v.value}rem;\n`;
      });
      navigator.clipboard
        .writeText(variablesText)
        .then(() => (this.successConvert = true));
    },
  },
};
</script>
<style lang="scss" scoped>
.container {
  max-width: 960px;
}

.main-wrap {
  display: flex;
  background: #04031c;
  height: 100vh;
  overflow: hidden;
  justify-content: center;
  align-items: center;
}

.main-title {
  display: flex;
  justify-content: center;
  align-content: center;
  font-size: 36px;
  color: #fff;
  margin-top: 30px;
}

.cal-content {
  margin-top: 20px;
  color: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px 2px rgba(53, 56, 139, 0.1);
  background-color: #fff;
  height: 750px;
  padding: 20px;
}

.input-content {
  display: flex;
  justify-content: flex-start;
  align-content: center;
  color: #04031c;
  font-weight: bold;
  align-items: center;
  font-size: 20px;
  margin-bottom: 20px;
}

.input {
  border: 1px #04031c solid;
  border-radius: 10px;
  outline: none;
  width: 100px;
  margin-right: 10px;
  padding: 5px;

  &.convert {
    width: 700px;
  }
}

.cal-label {
  margin-right: 10px;
  font-weight: bold;
  font-size: 20px;
}

.button-wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  .convert-button {
    background: #04031c;
    padding: 10px;
    border-radius: 10px;
    box-shadow: 0 0 10px 2px rgba(53, 56, 139, 0.1);
    color: #fff;
    letter-spacing: 1.5px;
    font-weight: bold;
  }
}
.convert-result {
  border: 1px solid #04031c;
  margin-bottom: 20px;
  border-radius: 10px;
  padding: 10px;
  max-height: 350px;
  overflow: auto;
  margin-top: 20px;
}
.result {
  color: #000;
  display: flex;
  justify-content: flex-start;
  gap: 20px;
  font-size: 20px;
  font-weight: bold;
}

.result-title {
  display: flex;
  justify-content: center;
  align-content: center;
  font-size: 36px;
  color: #000;
}

.scss-variables-convert-success {
  color: #04031c;
  font-size: 20px;
  font-weight: bold;
  margin-top: 20px;
}
</style>
