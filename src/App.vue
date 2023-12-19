<script setup lang="ts">
import { computed, ref } from "vue";

import ClipboardIcon from "@/components/icons/ClipboardIcon.vue";

const isResultCopied = ref<boolean>(false);

const style = ref<string>("solid");
const width = ref<string>("1");
const measureUnit = ref<string>("px");
const radius = ref<string>("");
const color = ref<string>("red");

const currentYear = computed(() => {
  return new Date(Date.now()).getFullYear();
});

const borderWidth = computed(() => {
  return `${width.value || 0}${measureUnit.value}`;
});

const properties = computed(() => {
  return `
.element  { 
             border-style:  ${style.value};
              border-width:  ${width.value}${measureUnit.value};
             border-radius:  ${radius.value};
             border-color:  ${color.value};
          }
`;
});

const copyResult = () => {
  copyText(properties.value);
};

const copyText = (value: string) => {
  isResultCopied.value = false;

  navigator.clipboard
    .writeText(value.toString())
    .then(() => {
      isResultCopied.value = true;
      setTimeout(() => (isResultCopied.value = false), 3000);
    })
    .catch((err) => console.error(err));
};
</script>

<template>
  <div class="wrapper">
    <header class="header">
      <h1 class="title">CSS Border Generator</h1>
    </header>

    <main class="container">
      <section class="cell options">
        <h2 class="options__title">Border options</h2>

        <ul>
          <li>
            <select v-model="style">
              <option value="none">none</option>
              <option value="hidden">hidden</option>
              <option value="dotted">dotted</option>
              <option value="dashed">dashed</option>
              <option value="solid">solid</option>
              <option value="double">double</option>
              <option value="groove">groove</option>
              <option value="ridge">ridge</option>
              <option value="inset">inset</option>
              <option value="outset">outset</option>
              <option value="initial">initial</option>
              <option value="inherit">inherit</option>
            </select>
          </li>

          <li>
            <input v-model="width" type="text" style="width: 30%" placeholder="border width" />

            <select v-model="measureUnit">
              <option value="cm">cm</option>
              <option value="mm">mm</option>
              <option value="in">in</option>
              <option value="px">px</option>
              <option value="pt">pt</option>
              <option value="pc">pc</option>
              <option value="em">em</option>
              <option value="ex">ex</option>
              <option value="ch">ch</option>
              <option value="rem">rem</option>
              <option value="vw">vw</option>
              <option value="vh">vh</option>
              <option value="vmin">vmin</option>
              <option value="vmax">vmax</option>
              <option value="%">%</option>
            </select>
          </li>

          <li>
            <input v-model="radius" type="text" placeholder="border radius" />
          </li>

          <li>
            <input v-model="color" type="text" placeholder="border color" />
          </li>
        </ul>
      </section>
      <section class="cell preview">
        <h2 class="preview__title">Live preview</h2>
        <div class="preview__element" :style="{ borderColor: color || 'black' }" />
      </section>
      <section class="cell snippet">
        <h2>Snippet output</h2>
        <ClipboardIcon class="clipboard" @click="copyResult" />
        <pre>
          <span>.element </span> <span class="brackets">{</span> 
            <span> border-style: </span> {{ style }};
            <span>  border-width: </span> {{ borderWidth }};
            <span> border-radius: </span> {{ radius || "0" }};
            <span> border-color: </span> {{ color }};
          <span class="brackets">}</span>
        </pre>
      </section>
    </main>

    <footer class="footer">
      <h5>
        Made by <a href="https://github.com/MHGuitarte">mhgdev</a> as part of florinpop17's
        <a href="https://github.com/florinpop17/app-ideas">app-ideas</a> - @{{ currentYear }}
      </h5>
    </footer>

    <div class="toast" :class="{ visible: isResultCopied }">
      <h3 class="copy-confirmation">Copied!</h3>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 100%;
  height: 100vh;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
}
.container {
  margin-inline: 4rem;
  height: 100%;
  width: 100%;
  display: grid;
  grid-template-areas:
    "options preview"
    "snippet snippet";
  column-gap: 1rem;
  align-items: center;
  justify-content: center;
}

.cell {
  width: 35vw;
}

.options {
  width: 100%;
  grid-area: options;
  display: flex;
  flex-direction: column;
}

.options__title {
  margin-bottom: 1rem;
}

.options > ul {
  list-style: none;
  gap: 1rem;
  display: flex;
  flex-direction: column;
}

.options > ul > li {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 0.25rem;
}

.preview {
  grid-area: preview;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.preview__title {
  width: 100%;
  text-align: center;
  margin: 1rem;
}

.preview__element {
  width: 160px;
  height: 160px;
  margin: 2rem;
  border-style: v-bind(style);
  border-width: v-bind(borderWidth);
  border-radius: v-bind(radius);
}

.snippet {
  grid-area: snippet;

  width: 100%;
  height: 100%;
  position: relative;
}

pre {
  margin: 2rem 16rem;
  padding: 0.5rem;
  background-color: rgba(0, 0, 0, 0.8);
  border-radius: 10px;
  color: white;
  border-left: 1rem solid green;
}

.clipboard {
  position: absolute;
  top: 15%;
  right: 28%;
}

.clipboard:hover {
  cursor: pointer;
}

pre > span {
  color: orange;
}

pre > span.brackets {
  color: yellow;
}

.footer {
  bottom: 0;
  position: absolute;
  width: 100%;
  display: flex;
}

.footer > h5 {
  margin: auto;
}

a {
  font-size: 0.75rem;
}

.toast {
  z-index: 10;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background-color: limegreen;
  color: white;
  font-weight: 700;

  visibility: hidden;
}

.toast.visible {
  visibility: visible;
  animation:
    fadein 0.5s,
    fadeout 0.5s 2.5s;
}

@keyframes fadein {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeout {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
</style>
