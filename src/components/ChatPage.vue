<template>
  <q-layout view="lHh Lpr lFf" container class="bg-white">
    <q-header class="bg-primary">
      <q-toolbar>
        <q-toolbar-title>Chat</q-toolbar-title>
        <q-btn flat v-close-popup round dense icon="close" />
      </q-toolbar>
    </q-header>
    <q-footer elevated>
      <q-toolbar>
        <q-form class="full-width" @submit="sendMsg" autocomplete="off">
          <div class="row">
            <q-input
            v-model="newMsg.text"
            ref= "newMessage"
            :bg-color="this.$q.dark.isActive ? 'bg-my-modal' : 'white'"
            outlined
            rounded
            label="Message"
            class="col-11"
            dense>
            </q-input>
            <div class="flex q-gutter-x-sm items-center q-ml-md">
            <div class="q-mx-lg">
                <q-input
                  ref="myFileInput"
                  style="display: none"
                  v-model="newMsg.files"
                  type="file"
                  accept=".doc,.docx,.xls,.xlsx,image/*"
                />
                <q-btn
                  flat
                  dense
                  round
                  size="md"
                  color="white"
                  icon="file_download"
                />
            </div>
            <q-btn
              round
              dense
              flat
              icon="send"
              color="white"
              type="submit"
              style="margin: 0"
              size="md"
            />
            </div>
          </div>
       </q-form>
      </q-toolbar>
    </q-footer>
    <q-page-container>
      <q-page class="page-chat flex">
        <div class="q-mt-sm column col justify-end">
          <div v-if="ScrollDownBtn" class="column items-center">
            <q-btn @click="scrollToLast" size="sm" color="purple" icon="keyboard_arrow_down" />
          </div>
          <q-scroll-area ref="chatScroll" class="col" @scroll="scroll">
            <q-chat-message
              v-for="(message, i) in messages"
              :key="i + message.text"
              :avatar="'https://cdn.quasar.dev/img/avatar3.jpg'"
              :name="message.from"
              :stamp="message.stamp"
              size="6"
              :sent="message.from !== 'me' ? true : false"
            >
              <div v-if="message.text">{{ message.text }}</div>
            </q-chat-message>
          </q-scroll-area>
        </div>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  data(){
    return{
      newMsg: {
        text: "",
        files: [],
      },
      messages: [],
      send: false,
      ScrollDownBtn: false
    }
  },
  methods:{
    scroll(el) {
      if (el.verticalPosition !== el.verticalSize - el.verticalContainerSize)
        this.ScrollDownBtn = true;
      else this.ScrollDownBtn = false;
    },
    scrollToLast() {
      let scrollArea = this.$refs.chatScroll;
      let scrollTarget = scrollArea.getScrollTarget();
      scrollArea.setScrollPosition('vertical',scrollTarget.scrollHeight, 0);
    },
    // Add new message in array messages
    sendMsg() {
      if (this.newMsg) {
        this.messages.push({
          text: this.newMsg.text,
          from: "me",
          stamp: new Date().toLocaleString("ru", this.locale),
          file: this.newMsg.files[0] ? this.newMsg.files[0] : false,
        });
        this.newMsg.text = "";
        this.newMsg.files = [];
      }
      setTimeout(()=>{
        this.scrollToLast()
      },10)
    },
  },
}
</script>