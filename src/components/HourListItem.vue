<template>
  <ion-item v-if="hour" class="list-item">
    <div slot="start" :class="currentClass"></div>
    <ion-label class="ion-text-wrap">
      <h2>{{ hour.hour }}h</h2>
    </ion-label>
    <ion-select
      placeholder="Je t'écoute"
      v-model="newLabel"
      @ionChange="updateHour()"
    >
      <ion-select-option v-for="label in labels" :key="label" :value="label">{{
        label
      }}</ion-select-option>
    </ion-select>
  </ion-item>
</template>

<script lang="ts">
import { IonItem, IonLabel, IonSelect, IonSelectOption } from "@ionic/vue";
import { defineComponent } from "vue";
import {LocalNotifications} from "@capacitor/local-notifications";

export default defineComponent({
  name: "HourListItem",
  components: {
    IonItem,
    IonLabel,
    IonSelect,
    IonSelectOption,
  },
  props: {
    hour: Object,
    canSend: Boolean,
  },
  methods: {
    isIos: () => {
      const win = window as any;
      return win && win.Ionic && win.Ionic.mode === "ios";
    },
    updateHour() {
      this.scheduleNotification(this.hour?.hour, this.hour?.label);
      this.$emit("updateHour", this.newLabel, this.hour?.hour);
    },
    async scheduleNotification(hour: number, body: string) {
      await LocalNotifications.schedule({
        notifications: [
          {
            body: "Au boulot il est " + hour + "h et arrête de manger tes crottes de nez !",
            title: body,
            id: hour,
            schedule:{
                on:{
                  hour: hour,
                  minute: 0
                }
              }
          },
        ],
      });
    },
  },
  data() {
    return {
      currentClass: "",
      labels: [
        "Modelisation 3D",
        "Schema Electronique",
        "Piano",
        "Lecture",
        "Impression 3D",
        "Atelier",
        "Maintenance",
        "Programmation",
        "Création Video",
        "Musique",
        "Enregistrement Audio",
        "Sport",
        "Fabrication PCB",
        "Dessin",
        "Ménage",
        "Administratif",
        "Post-it",
        "Autre",
      ],
      newLabel: this.hour?.label,
    };
  },
  created() {
    const date = new Date();
    const currentHour = date.getHours();
    this.currentClass = "dot dot-after";
    if (currentHour > this?.hour?.hour) {
      this.currentClass = "dot dot-before";
    } else if (currentHour == this?.hour?.hour) {
      this.currentClass = "dot dot-current";
    }
    this.scheduleNotification(this.hour?.hour, this.hour?.label);
    setInterval(() => {
      const date = new Date();
      const currentHour = date.getHours();
      this.currentClass = "dot dot-after";
      if (currentHour > this?.hour?.hour) {
        this.currentClass = "dot dot-before";
      } else if (currentHour == this?.hour?.hour) {
        this.currentClass = "dot dot-current";
      }
    }, 60000);
  },
});
</script>

<style scoped>
.list-item {
  --padding-start: 0;
  --inner-padding-end: 0;
}

.list-item ion-label {
  margin-top: 12px;
  margin-bottom: 12px;
}

.list-item h2 {
  font-weight: 600;
  margin: 0;
}

.list-item p {
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  width: 95%;
}

.list-item .date {
  float: right;
  align-items: center;
  display: flex;
}

.list-item ion-icon {
  color: #c9c9ca;
}

.list-item ion-note {
  font-size: 15px;
  margin-right: 8px;
  font-weight: normal;
}

.list-item ion-note.md {
  margin-right: 14px;
}

.list-item .dot {
  display: block;
  height: 12px;
  width: 12px;
  border-radius: 50%;
  align-self: start;
  margin: 16px 10px 16px 16px;
}

.list-item .dot-before {
  background: var(--ion-color-secondary);
}
.list-item .dot-after {
  background: var(--ion-color-warning);
}
@keyframes blink {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
.list-item .dot-current {
  background: var(--ion-color-primary);
  animation: blink 2s infinite;
}
ion-select {
  max-width: 100% !important;
}
</style>