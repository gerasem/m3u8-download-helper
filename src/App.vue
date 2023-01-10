<script setup>
import { ref } from "vue";
const textarea =
  ref(`https://playlist.servicecdn.ru/player/b49e76f15b90f375c7d7710decc213cb/00d6572b09b386d90dcb1ae2baf20300/master.m3u8?user-cdn=gcore&acc-id=0&user-id=136457026&loc-mode=ru&version=5%3A2%3A1%3A0%3A2%3Agcore&consumer=vod&jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyLWlkIjoxMzY0NTcwMjYsImNvbnN1bWVyIjoidm9kIn0.hiFt-yf8Ut3WmV9jbfb2IHucqTsCwVff7jFvCDLdYQk&gzipoff=1
https://player02.getcourse.ru/player/dead81caf4d477a62370c07e793c6f47/d3e5ee7e06934bcb61b93177c4456bbf/master.m3u8?user-cdn=gcore&acc-id=0&user-id=136457026&loc-mode=ru&version=5%3A0%3A1%3A0%3A0%3Agcore&consumer=vod&jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyLWlkIjoxMzY0NTcwMjYsImNvbnN1bWVyIjoidm9kIn0.hiFt-yf8Ut3WmV9jbfb2IHucqTsCwVff7jFvCDLdYQk
https://playlist.servicecdn.ru/player/5ed802a53e54d860ece76ce6e600da94/c78bc57bee63f92d1508c2ada7779b85/master.m3u8?user-cdn=gcore&acc-id=0&user-id=136457026&loc-mode=ru&version=5%3A2%3A1%3A0%3A2%3Agcore&consumer=vod&jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyLWlkIjoxMzY0NTcwMjYsImNvbnN1bWVyIjoidm9kIn0.hiFt-yf8Ut3WmV9jbfb2IHucqTsCwVff7jFvCDLdYQk&gzipoff=1`);
const error = ref("");
const separator = ref("\n");
const separatorList = [
  { name: "Enter", value: "\n" },
  { name: "Comma", value: "," },
  { name: "Semicolon", value: ";" },
];
function generate() {
  setTimeout(() => {
    error.value = "";
  }, 2000);
  if (!textarea.value) {
    error.value = "textarea is empty";
  }
  if (!textarea.value.includes("http") && !textarea.value.includes(".m3u8")) {
    error.value = "no .m3u8 link found";
  }
  const links = textarea.value.split(separator.value);
  console.log(links);
}
function removeNotification() {
  error.value = "";
}
function changeSeparator(value) {
  separator.value = value;
}
</script>

<template>
  <section class="hero is-primary is-fullheight">
    <div class="hero-body">
      <div class="container has-text-centered">
        <div
          v-if="error"
          class="notification is-danger"
        >
          <button
            @click="removeNotification()"
            class="delete"
          ></button>
          {{ error }}
        </div>
        <p class="title">.m3u8 Download Helper</p>
        <p class="subtitle">Paste Links here ⬇️</p>
        <textarea
          class="textarea is-primary"
          v-model="textarea"
          placeholder="Link with .m3u8 File"
        ></textarea>
        <div class="has-text-left mt-4">
          <div class="field">
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
        </div>
        <button
          class="mt-4 button is-info is-rounded is-medium"
          @click="generate"
        >
          Generate command
        </button>
      </div>
    </div>
  </section>
</template>

<style scoped></style>
