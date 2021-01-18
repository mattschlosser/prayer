<template>
  <v-container>
    <h1>Prayer Items</h1>
    <v-banner/>
    <v-card  style="padding: 25px; margin: 15px auto;">
      <v-text-field label="Title" v-model="title">
      </v-text-field>
      <v-textarea label="Prayer" v-model="prayer">
      </v-textarea>
    <v-btn depressed block @click="addItem">Add Item</v-btn>
    </v-card>
    <h2>Unaswered</h2>
    <v-banner />
    <div class="d-flex flex-wrap  flex-row justify-space-between">
      <template v-for="(prayer, i) in prayers">
        <v-card :key="i" min-width="320" class="mt-3">
          <v-card-title>
            {{ prayer.title }}
          </v-card-title>
          <v-card-subtitle>
            {{ prayer.date }}
          </v-card-subtitle>
          <v-card-text>
            {{ prayer.prayer}}
          </v-card-text>
          <v-card-actions>
            <v-btn text color="red" @click="removeUnanswered(i)">Delete</v-btn>
            <v-btn text @click="setAsAnswered(i)">Mark as Answered</v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </div>
    <div>
      <h2>Answered</h2>
      <v-banner />
    <div class="d-flex flex-wrap  flex-row justify-space-between">
      <template v-for="(prayer, i) in answered">
        <v-card :key="i" min-width="320" class="mt-3">
          <v-card-title>
            {{ prayer.title }}
          </v-card-title>
          <v-card-subtitle>
            {{ prayer.date }}
          </v-card-subtitle>
          <v-card-text>
            {{ prayer.prayer}}
          </v-card-text>
          <v-card-actions>
            <v-btn text color="red" @click="removeAnswered(i)">Delete</v-btn>
          </v-card-actions>
        </v-card>
      </template>
    </div>
    </div>
  </v-container>
</template>

<script>
  export default {
    name: 'HelloWorld',
    created() {
      this.deserialize()
    }, 
    data: () => ({
      prayers: [],
      answered: [],
      prayer: '',
      title: ''
    }),
    methods: {
      addItem() {
        let {prayer, title} = this;
        this.prayers.unshift({prayer, title, date: new Date()})
        this.prayer = '';
        this.title = '';
        this.serialize();
      }, 
      removeAnswered(i) {
        this.answered.splice(i, 1);
        this.serialize()
      }, 
      removeUnanswered(i) {
        this.prayers.splice(i, 1);
        this.serialize();
      }, 
      setAsAnswered(i) {
        this.answered.push({...this.prayers.splice(i, 1)[0], answered: new Date()});
        this.serialize();
      },
      serialize() {
        localStorage.prayers = JSON.stringify(this.prayers);
        localStorage.answered = JSON.stringify(this.answered);
      },
      deserialize() {
        this.prayers = JSON.parse(localStorage.prayers)
        this.answered = JSON.parse(localStorage.answered);
      }
    }
  }
</script>
