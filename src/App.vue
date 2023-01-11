<script setup>
import { ref } from "vue";
const textarea = ref("");
const error = ref("");
const separator = ref("\n");
const lession = ref("");
const success = ref("");
const separatorList = [
  { name: "Enter", value: "\n" },
  { name: "Comma", value: "," },
  { name: "Semicolon", value: ";" },
];
function generate() {
  setTimeout(() => {
    if (error.value) {
      error.value = "";
    }
  }, 2000);
  if (!textarea.value) {
    error.value = "Textarea is empty";
    return
  }
  if (!textarea.value.includes("http") && !textarea.value.includes(".m3u8")) {
    error.value = "No .m3u8 link found";
    return
  }
  const links = textarea.value.split(separator.value);
  console.log(links);
  let result = "";
  for (let i = 0; i < links.length; i++) {
    if (links[i]) {
      result += `ffmpeg -i "${links[i]}" -c copy -bsf:a aac_adtstoasc "${
        lession.value
      }-${i + 1}.mp4";`;
    }
  }
  if (result) {
    const el = document.createElement("textarea");
    el.value = result;
    document.body.appendChild(el);
    el.select();
    document.execCommand("copy");
    document.body.removeChild(el);
    success.value =
      "Done! Commands have been copied to the clipboard. Please open your Terminal and paste your generated commands";
    setTimeout(() => {
      success.value = "";
    }, 3000);
  }
}
</script>

<template>
  <section class="hero is-fullheight">
    <div class="hero-body">
      <div class="container has-text-centered">
        <div
          v-if="error"
          class="notification is-danger"
        >
          <button
            @click="error = ''"
            class="delete"
          ></button>
          {{ error }}
        </div>
        <div
          v-if="success"
          class="notification is-primary"
        >
          <button
            @click="success = ''"
            class="delete"
          ></button>
          {{ success }}
        </div>
        <p class="title">.m3u8 Download Helper</p>
        <p class="subtitle">Paste Links here ⬇️</p>
        <textarea
          class="textarea"
          v-model="textarea"
          placeholder="Link with .m3u8 File"
        ></textarea>
        <div class="has-text-left mt-4 columns">
          <div class="field column">
            <label class="label">Separator</label>
            <div class="control">
              <div class="select">
                <select v-model="separator">
                  <option
                    v-for="option in separatorList"
                    :key="option.name"
                    :value="option.value"
                  >
                    {{ option.name }}
                  </option>
                </select>
              </div>
            </div>
          </div>

          <div class="field column">
            <label class="label">Lession name</label>
            <div class="control">
              <input
                class="input"
                type="text"
                v-model="lession"
                placeholder="Lession name"
              />
            </div>
          </div>
        </div>
        <button
          class="mt-4 button is-primary is-rounded is-medium"
          @click="generate"
        >
          Generate command
        </button>
      </div>
    </div>
  </section>
</template>

<style scoped></style>
