<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Jim's Life Sequencer</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Jim's Life Sequencer</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-list v-if="hours && hours?.length > 0">
        <HourListItem
          v-for="hour in hours"
          :key="hour.hour"
          :hour="hour"
          @updateHour="updateHour"
          :canSend="canSend"
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
  IonTitle,
  IonToolbar,
} from "@ionic/vue";
import { defineComponent } from "vue";
import { Storage } from "@ionic/storage";
import HourListItem from "@/components/HourListItem.vue";

export default defineComponent({
  name: "Home",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    IonList,
    HourListItem,
  },
  async created() {
    this.store.create().then(() => {
      this.store.get("hours").then((hours: any) => {
        this.hours = JSON.parse(hours);
        if (!this.hours || this?.hours.length == 0) {
          this.hours = [
            {
              hour: 9,
              label: "Modelisation 3D",
            },
            {
              hour: 10,
              label: "Schema Electronique",
            },
            {
              hour: 11,
              label: "Piano",
            },
            {
              hour: 12,
              label: "Lecture",
            },
            {
              hour: 13,
              label: "Impression 3D",
            },
            {
              hour: 14,
              label: "Atelier",
            },
            {
              hour: 15,
              label: "Maintenance",
            },
            {
              hour: 16,
              label: "Programmation",
            },
            {
              hour: 17,
              label: "Création Video",
            },
            {
              hour: 18,
              label: "Musique",
            },
            {
              hour: 19,
              label: "Sport",
            },
          ];
          this.store.set("hours", JSON.stringify(this.hours));
        }
      });
    });
  },
  data() {
    return {
      hours: [] as any,
      store: new Storage(),
    };
  }
});
</script>

<style scoped>
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>