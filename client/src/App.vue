<script setup>
import axios from "axios";
import { ref } from "vue";

const msgs = ref([]);
const msgContent = ref("");

function sendMessage() {
  if (msgContent.value == "") return;

  createMsg(msgContent.value);
  getResponse(msgContent.value);
  msgContent.value = "";
}

function createMsg(msg) {
  let id = 0;
  if (msgs.value[msgs.value.length - 1]) {
    id = msgs.value[msgs.value.length - 1].id + 1;
  }
  msgs.value.push({
    id: id,
    msg: msg,
  });
}

async function getResponse(msg) {
  const postData = {
    msg: msg,
  };
  const { data } = await axios.post("http://localhost:7000/chat", postData);
  const { response } = data;
  createMsg(response);
}
</script>

<template>
  <div class="flex flex-col h-screen bg-gray-400 rounded-md">
    <!-- Chat messages container -->
    <div class="flex-grow px-4 pt-6 pb-2" style="height: 80vh; overflow-y: auto;">
      <div class="flex flex-col space-y-4" v-for="msg in msgs" :key="msg.id">
        <!-- BOT message -->
        <div class="flex justify-end items-start p-2" v-if="msg.id % 2 == 0">
          <div class="ml-4">
            <div class="bg-violet-500 text-white p-4 rounded-lg max-w-xs">
              <p class="text-gray-600">{{ msg.msg }}</p>
            </div>
          </div>
          <div class="flex-shrink-0 p-2">
            <img
              class="w-10 h-10 rounded-full"
              src="https://via.placeholder.com/50"
              alt="Avatar"
            />
          </div>
        </div>

        <!-- end -->

        <!-- USER message -->
        <div class="flex justify-start items-start" v-else>
          <div class="flex-shrink-0">
            <img
              class="w-10 h-10 rounded-full"
              src="https://plus.unsplash.com/premium_photo-1677094310956-7f88ae5f5c6b?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=880&q=80"
              alt="Avatar"
            />
            <span class="text-black font-semibold text-xs">BOT</span>
          </div>
          <div class="ml-4">
            <div class="bg-white p-4 rounded-lg max-w-xs">
              <div class="">
                <p class="text-gray-800 font-medium">{{ msg.msg }}</p>
              </div>
            </div>
          </div>
        </div>
        <!-- end -->
      </div>
    </div>

    <!-- Chat input container -->
    <div
      class="flex-shrink-0 px-4 py-2 bg-white border-t border-gray-200 rounded-md"
    >
      <div class="flex items-center">
        <form @submit.prevent="sendMessage()">
          <input
            v-model="msgContent"
            id="createMsg"
            class="flex-grow px-4 py-2 mr-2 text-gray-700 bg-gray-100 rounded-full focus:outline-none"
            type="text"
            placeholder="Type your message..."
          />
          <button
            class="flex-shrink-0 px-4 py-2 text-white bg-violet-500 rounded-full hover:bg-violet-600 focus:outline-none"
          >
            Send
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
