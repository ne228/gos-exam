<template>
  <v-container>
    <v-card class="cards mt-2 pa-2">
      <h3>
        Можно потестить как будет выглядит ваш стол на госах. Попытался сделать
        так чтобы влезло на мобилки. Лучше развернуть телефон в альбомную
        ориентацию
      </h3>
      <div class="tables">
        

      <div class="table">
        <v-row v-for="row in 3" :key="row" class="my-2">
          <v-col v-for="col in 5" :key="col">
            <v-card
              color = 'primary'
              v-if="cards.includes(row + '-' + col)"
              @click="removeCard(row + '-' + col)"
              class="pa-1 card"
            >
              <p class="text-center">
                {{ showNumbers ? (row - 1) * 10 + col : "" }}
              </p>
            </v-card>
          </v-col>
      </v-row>
      </div>
      <div class="table">
        <v-row v-for="row in 3" :key="row" class="my-2">
          <v-col v-for="col in 5" :key="col">
            <v-card
              color = 'primary'
              v-if="cards.includes(row + '-' + (col +5))"
              @click="removeCard(row + '-' + (col+5))"
              class="pa-1 card"
            >
              <p class="text-center">
                {{ showNumbers ? (row - 1) * 10 + (col+5) : "" }}
              </p>
            </v-card>
          </v-col>
      </v-row>
      </div>
      </div>

    </v-card>

    <v-card class="pa-5 mt-2">
      <h3>Настройки</h3>
      <v-switch
        v-model="showNumbers"
        color="primary"
        label="Показывать номера на карточках"
      ></v-switch>
      <v-switch v-model="showLog" label="Показывать лог"></v-switch>
    </v-card>

    <v-card class="mt-2 pa-2" v-if="showLog">
      <h3 class="mb-2 pa-2">Лог</h3>
      <div v-for="message in getMessages" :key="message">
        <p>{{ message }}</p>
      </div>
    </v-card>

    <v-dialog v-model="settingsDialog" persistent max-width="290">
      <v-card>
        <v-card-title>Настройки</v-card-title>
        <v-card-text>
          <v-switch
            v-model="showNumbers"
            label="Показывать номера на карточках"
          ></v-switch>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" text @click="settingsDialog = false"
            >Готово</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    cards: Array.from(
      { length: 30 },
      (_, i) => Math.floor(i / 10) + 1 + "-" + ((i % 10) + 1)
    ),
    settingsDialog: false,
    shiftCards: false,
    showNumbers: true,
    messages: [],
    countMessage: 0,
    showLog: true,
  }),
  computed: {
    getMessages() {
      // var res = this.messages.reverse();

      return this.messages;
    },
  },

  methods: {
    removeCard(card) {
      const parts = card.split("-");
      var cardNum = (parts[0] - 1) * 10 + parseInt(parts[1]);
      this.countMessage += 1;
      console.log(this.countMessage + ". Выбран вариант №" + cardNum);
      this.messages.push(
        this.countMessage.toString() + ". Выбран вариант №" + cardNum
      );
      this.cards = this.cards.filter((c) => c !== card);
    },
    toggleSettings() {
      this.settingsDialog = true;
    },
    toggleLog() {
      this.showLog = !this.showLog;
    },
  },
};
</script>
<style>
.cards {
  width: 100%;
}
.card {
  width: 3.1em;
  height: 2em;
  font-size: 1em;
  margin: 1%;
}
.tables{
  display:flex;
  flex-direction: row;
}
.table{
  width: 45%;
  border:1px solid black;
  margin:1%;
  border-radius: 10px;
  padding: 1%;
}
</style>
