<template>
  <q-page class="flex flex-center">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    >
    <q-page-sticky position="bottom-right" :offset="[18, 18]">
       <q-fab v-model="openDialog" color="secondary" push icon="chat" direction="right">
        <q-dialog 
          v-model="openDialog"
          :maximized="true"
        >
          <ChatPage />
        </q-dialog>
      </q-fab>

    </q-page-sticky>
  </q-page>
</template>

<script>
import ChatPage from '../components/ChatPage.vue'
export default {
  name: 'PageIndex',
  components: { ChatPage },
  data(){
    return{
      openDialog: false,
    }
  },
  methods:{
  }
}
</script>
