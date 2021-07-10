<template>
  <div
    id="ui-container"
    :class="{
      closed: !windowOpen,
    }"
    :style="{
      '--scale': cssConf.scale,
      '--border-radius-scale': cssConf.borderRadScale,
      '--bg-primary': cssConf.bgPrimary,
      '--bg-secondary': cssConf.bgSecondary,
      '--bg-tertiary': cssConf.bgTertiary,
      '--color-primary': cssConf.colorPrimary,
      '--color-secondary': cssConf.colorSecondary,
      '--color-error': cssConf.colorError,
      '--text-primary': cssConf.textPrimary,
      '--text-secondary': cssConf.textSecondary,
      '--text-placeholder': cssConf.textPlaceholder,
    }"
  >
    <div class="identity-header">
      <span class="welcomer">{{ lang.welcomeText }}</span
      ><br /><span class="server-name">{{ lang.serverName }}</span>
    </div>
    <div class="name-input-container">
      <div class="input-element">
        <label for="name">{{ lang.labelName }}</label
        ><br /><input
          type="text"
          name="name"
          id="name"
          class="name-input"
          :placeholder="lang.placeholderName"
          autocomplete="off"
          v-model="characterData.name.first"
        />
      </div>
      <div class="input-element">
        <label for="lastname">{{ lang.labelLastName }}</label
        ><br /><input
          type="text"
          name="lastname"
          id="lastname"
          class="name-input"
          :placeholder="lang.placeholderLastName"
          autocomplete="off"
          v-model="characterData.name.last"
        />
      </div>
    </div>

    <div class="birthdate-input-container">
      <div class="input-element">
        <label for="birthdate">{{ lang.labelBirthdate }}</label
        ><br /><input
          type="date"
          name="birthdate"
          id="birthdate"
          :max="today"
          v-model="characterData.birthdate"
        />
      </div>
    </div>
    <div class="gender-selecter">
      <label>{{ lang.labelGenderSelector }}</label>
      <RadioGroup :buttons="selectGender"></RadioGroup>
    </div>
    <div class="signup-btn" @click="submit">{{ lang.labelSignUp }}</div>
  </div>
</template>

<script>
import RadioGroup from "./components/RadioGroup.vue";
export default {
  name: "App",
  components: {
    RadioGroup,
  },
  data() {
    return {
      lang: {
        welcomeText: "Welcome to",
        serverName: "SampleServer",
        placeholderName: "Name",
        placeholderLastName: "Last name",
        placeholderBirthdate: "Birthdate",
        labelName: "Name:",
        labelLastName: "Last name:",
        labelBirthdate: "Birthdate:",
        labelGenderSelector: "Gender: ",
        labelSignUp: "Create character",
      },
      cssConf: {
        scale: 1,
        borderRadScale: 1,
        bgPrimary: "#4a4a55",
        bgSecondary: "#383844",
        bgTertiary: "#242438",
        colorPrimary: "#23bf7c",
        colorSecondary: "#299465",
        colorError: "#cc3333",
        textPrimary: "#f7f7ff",
        textSecondary: "#dfdfef",
        textPlaceholder: "#878797",
      },
      selectGender: [
        {
          index: 0,
          data: "Male",
        },
        {
          index: 1,
          data: "Female",
        },
        {
          index: 2,
          data: "Non-binary",
        },
      ],
      characterData: {
        name: {
          first: "",
          last: "",
        },
        birthdate: "",
        gender: "",
      },
      windowOpen: false,
    };
  },
  computed: {
    today() {
      let date = new Date();
      let day = date.getDate().toLocaleString("en-US", {
        minimumIntegerDigits: 2,
        useGrouping: false,
      });
      let month = (date.getMonth() + 1).toLocaleString("en-US", {
        minimumIntegerDigits: 2,
        useGrouping: false,
      });
      let year = date.getFullYear().toLocaleString("en-US", {
        minimumIntegerDigits: 4,
        useGrouping: false,
      });
      let dateFormatted = `${day}/${month}/${year}`;
      return dateFormatted;
    },
  },
  methods: {
    submit() {
      fetch("https://gm_identity/create", {
        method: "post",
        body: JSON.stringify(this.characterData),
      });
      this.closeWindow();
    },
  },
  mounted() {
    fetch("https://gm_identity/lang", {
      method: "post",
    })
      .then((res) => res.json())
      .then((data) => {
        this.lang = { ...data };
      });
    fetch("https://gm_identity/css", { method: "post" })
      .then((res) => res.json())
      .then((data) => {
        this.cssConf = { ...data };
      });
    window.addEventListener("message", (e) => {
      let data = e.data;
      if (data.type === "GM-IdentityGenderRadioGroupSelectionBroadcast") {
        this.characterData.gender = data.buttonSelected;
      } else if (data.type === "enableui") {
        this.windowOpen = data.enable;
      }
    });
    this.characterData.birthdate = this.today;
  },
};
</script>

<style lang="scss">
#ui-container {
  //scale
  --scale: 1;
  --border-radius-scale: 1;

  // colors
  --bg-primary: #4a4a55;
  --bg-secondary: #383844;
  --bg-tertiary: #242438;
  --color-primary: #23bf7c;
  --color-secondary: #299465;
  --color-error: #cc3333;
  --text-primary: #f7f7ff;
  --text-secondary: #dfdfef;
  --text-placeholder: #878797;

  user-select: none;

  height: calc(60vh * var(--scale));
  width: calc(55vh * var(--scale));

  background-color: var(--bg-primary);
  border-radius: calc(2vh * var(--border-radius-scale));
  border: 0.1vh solid var(--bg-secondary);
  .identity-header {
    font-family: Helvetica, Arial, sans-serif;
    height: calc(10vh * var(--scale));
    text-align: center;
    user-select: none;
    .welcomer {
      font-size: calc(3vh * var(--scale));
      line-height: calc(4vh * var(--scale));
      color: var(--text-secondary);
      position: relative;
      top: 1vh;
    }
    .server-name {
      font-size: calc(4vh * var(--scale));
      line-height: calc(6vh * var(--scale));
      color: var(--text-primary);
    }
    border-bottom: calc(0.1vh * var(--scale)) solid var(--bg-secondary);
  }

  label {
    display: inline-block;
    font-size: calc(2.2vh * var(--scale));
    margin-bottom: calc(0.2vh * var(--scale));
    font-family: Helvetica, Arial, sans-serif;
    color: var(--text-primary);
  }
  input {
    height: calc(4vh * var(--scale));
    width: calc(45vh * var(--scale));

    background-color: var(--bg-primary);
    border: calc(0.3vh * var(--scale)) solid var(--bg-secondary);
    border-radius: calc(1vh * var(--scale));
    text-indent: calc(0.5vh * var(--scale));
    font-size: calc(2.3vh * var(--scale));
    font-family: Helvetica, Arial, sans-serif;
    color: var(--text-primary);
    transition: border 0.2s;
    background-color: var(--bg-secondary);
    &:focus {
      outline: none;
      border: calc(0.3vh * var(--scale)) solid var(--color-primary) !important;
      transition: border 0.1s;
    }
    &::placeholder {
      color: var(--text-placeholder);
    }
    &:hover {
      border: calc(0.3vh * var(--scale)) solid var(--bg-tertiary);
    }
    &[type="date"] {
      user-select: none;
      &::-webkit-calendar-picker-indicator {
        background-image: url("./assets/calendar.svg");
        background-size: auto cover;
        &:focus {
          outline: none;
        }
      }
    }
  }
  .name-input-container {
    position: relative;
    top: 2.5vh;
    display: grid;
    place-items: center;
    gap: calc(2.5vh * var(--scale));
  }

  .birthdate-input-container {
    margin-left: calc(5vh * var(--scale));
    position: relative;
    top: calc(5vh * var(--scale));
    #birthdate {
    }
  }

  .gender-selecter {
    position: relative;
    top: calc(7.5vh * var(--scale));
    left: calc(5vh * var(--scale));
  }
  .signup-btn {
    background-color: var(--bg-primary);
    transition: background-color 0.3s;
    border: calc(0.3vh * var(--scale)) solid var(--color-primary);
    &:hover {
      background-color: var(--color-primary);
      transition: background-color 0.15s;
    }
    border-radius: calc(1vh * var(--scale));
    text-indent: calc(0.5vh * var(--scale));
    font-size: calc(2.3vh * var(--scale));
    font-family: Helvetica, Arial, sans-serif;
    color: var(--text-primary);
    position: relative;
    top: calc(10vh * var(--scale));
    width: calc(45vh * var(--scale));
    left: calc(5vh * var(--scale));
    height: calc(4vh * var(--scale));
    line-height: calc(3.5vh * var(--scale));
    text-align: center;
    cursor: pointer;
  }
  &.closed {
    opacity: 0;
    transform: scale(1.1);
    transition: all 0.2s;
  }
}
* {
  box-sizing: border-box;
}
html,
body {
  height: 100%;
  width: 100%;
  display: grid;
  place-items: center;
  margin: 0;
  padding: 0;
}
</style>
