<template>
  <v-container>
    <!-- AppBar -->
    <v-app-bar color="primary" dark>
      <v-toolbar-title>
        <div class="d-flex align-center">
        <v-img alt="supehero-logo" class="shrink mr-2" contain src="../assets/superhero-icon.png"
          transition="scale-transition" width="30" />
          <span v-if="isWideScreen">Super Heroes</span>
        </div>
        <v-spacer></v-spacer>
      </v-toolbar-title>
    </v-app-bar>

    <!-- Tabla -->
    <v-row>
      <v-col cols="12" md="6">
        <v-data-table
          :headers="headers"
          :items="items"
          :items-per-page="1"
          class="elevation-1"
        >
          <template slot="item.actions" slot-scope="{ item }">
            <v-icon small @click="editItem(item)">
              mdi-pencil
            </v-icon>
            <v-icon small @click="deleteItem(item)">
              mdi-delete
            </v-icon>
          </template>
        </v-data-table>
      </v-col>
    </v-row>

    <!-- Formulario -->
    <v-row>
      <v-col cols="12" md="6">
        <v-form ref="form" v-model="isFormValid" lazy-validation>
          <v-text-field v-model="superHero" label="Super Héroe" required></v-text-field>
          <v-text-field v-model="alterEgo" label="Alter ego" required></v-text-field>
          <v-slider
            v-model="power"
            :max="5"
            :step="1"
            label="Poder"
            ticks
            thumb-label
            required
          ></v-slider>
          <v-textarea v-model="description" label="Descripción" required></v-textarea>
          <v-btn color="green" @click="submitForm">
            {{ isEditing ? 'Actualizar' : 'Guardar' }}
          </v-btn>
          <v-btn color="grey" @click="resetForm">
            Cancelar
          </v-btn>
        </v-form>
      </v-col>
    </v-row>

    <!-- Botones con íconos -->
    <v-row>
      <v-col cols="12" md="6" class="text-center">
        <v-btn icon>
          <v-icon>mdi-home</v-icon>
        </v-btn>
        <v-btn icon>
          <v-icon>mdi-account</v-icon>
        </v-btn>
        <v-btn icon>
          <v-icon>mdi-camera</v-icon>
        </v-btn>
        <v-btn icon>
          <v-icon>mdi-logout</v-icon>
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      isFormValid: false,
      superHero: '',
      alterEgo: '',
      power: 0,
      description: '',
      isEditing: false,
      editedIndex: -1,
      headers: [
        { text: 'Super Héroe', value: 'superHero' },
        { text: 'Alter ego', value: 'alterEgo' },
        { text: 'Poder', value: 'power' },
        { text: 'Descripción', value: 'description' },
        { text: 'Acciones', value: 'actions', sortable: false },
      ],
      items: [],
      isWideScreen: window.innerWidth > 600,
    };
  },
  methods: {
    submitForm() {
      if (this.$refs.form.validate()) {
        if (this.isEditing) {
          Object.assign(this.items[this.editedIndex], {
            superHero: this.superHero,
            alterEgo: this.alterEgo,
            power: this.power,
            description: this.description,
          });
          this.isEditing = false;
          this.editedIndex = -1;
        } else {
          this.items.push({
            superHero: this.superHero,
            alterEgo: this.alterEgo,
            power: this.power,
            description: this.description,
          });
        }
        this.resetForm();
      }
    },
    editItem(item) {
      this.editedIndex = this.items.indexOf(item);
      this.superHero = item.superHero;
      this.alterEgo = item.alterEgo;
      this.power = item.power;
      this.description = item.description;
      this.isEditing = true;
    },
    deleteItem(item) {
      const index = this.items.indexOf(item);
      if (index > -1) {
        this.items.splice(index, 1);
      }
    },
    resetForm() {
      this.superHero = '';
      this.alterEgo = '';
      this.power = 0;
      this.description = '';
      this.isEditing = false;
      this.editedIndex = -1;
    },
    handleResize() {
      this.isWideScreen = window.innerWidth > 600;
    }
  },
  mounted() {
    window.addEventListener('resize', this.handleResize);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.handleResize);
  },
};
</script>

<style scoped>
@media (max-width: 600px) {
  .v-toolbar-title span {
    display: none;
  }
}
</style>
