<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title @click="getUnreadMessage('asd', 'asdasd');">Inbox {{ unreadChats }}</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-refresher slot="fixed" @ionRefresh="refresh($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>

      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Inbox</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-list>
        <MessageListItem
          v-for="message in chats"
          :key="message.id"
          :message="message"
        />
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonContent,
  IonHeader,
  IonList,
  IonPage,
  IonRefresher,
  IonRefresherContent,
  IonTitle,
  IonToolbar,
} from "@ionic/vue";
import MessageListItem from "@/components/MessageListItem.vue";
import { defineComponent } from "vue";
import { getMessages } from "@/data/messages";
import axios from "axios";
import { stringifyQuery } from "node_modules/vue-router/dist/vue-router";

export default defineComponent({
  name: "Home",
  data() {
    return {
      messages: getMessages(),
      chats: [],
      unreadChats: 0,
    };
  },
  methods: {
    refresh: (ev: CustomEvent) => {
      setTimeout(() => {
        ev.detail.complete();
      }, 3000);
    },
    getUnreadMessage: (from: any, to: any) => {
      axios
        .get("http://127.0.0.1:8000/api/countUnreadMessage/jerald/124.105.241.204")
        .then((response) => {
          console.log("success");
          console.log(response.data);
          this.unreadChats = response.data;
          return response.data;
        })
        .catch((error) => {
          console.log("error");
          console.log(error.data);
          return undefined;
        });
    },
  },
  components: {
    IonContent,
    IonHeader,
    IonList,
    IonPage,
    IonRefresher,
    IonRefresherContent,
    IonTitle,
    IonToolbar,
    MessageListItem,
  },
  mounted() {
    console.log("start sa message");
    console.log(this.messages);
    console.log("end sa message");
    axios
      .get("http://127.0.0.1:8000/api/getChatsList")
      .then((response) => {
        console.log("chats ni");
        this.chats = response.data;
        console.log(this.chats);
      })
      .catch((error) => {
        console.log("error");
        console.log(error);
      });
  },
});
</script>
