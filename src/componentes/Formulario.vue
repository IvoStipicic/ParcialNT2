<template lang="html">
  <div class="jumbotron">
    <h2>Formulario ingreso de gastos</h2>

    <vue-form :state="formState" @submit.prevent="enviar()">
      <validate tag="div">
        <label for="nombre">Nombre</label>
        <input
          class="form-control"
          type="text"
          id="nombre"
          name="nombre"
          v-model.trim="formData.nombre"
          autocomplete="off"
          :minlength="nombreMin"
          :maxlength="nombreMax"
          no-espacios
          required
        />
        <field-messages name="nombre" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">
            Campo requerido
          </div>
          <div slot="no-espacios" class="alert alert-dark mt-1">
            No se permiten espacios
          </div>
          <div slot="minlength" class="alert alert-dark mt-1">
            Este campo requiere un nombre de minimo {{ nombreMin }} letras
          </div>
          <div
            v-if="formData.nombre.length == nombreMax"
            class="alert alert-dark mt-1"
          >
            El nombre debe tener como máximo {{ nombreMax }} caracteres
          </div>
        </field-messages>
      </validate>

      <validate tag="div">
        <label for="descripcion">Descripcion</label>
        <input
          class="form-control"
          type="text"
          id="descripcion"
          name="descripcion"
          v-model.trim="formData.descripcion"
          autocomplete="off"
          required
        />
        <field-messages name="descripcion" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">
            Campo requerido
          </div>
        </field-messages>
      </validate>

      <validate tag="div">
        <label for="importe">Importe</label>
        <input
          class="form-control"
          type="number"
          id="importe"
          name="importe"
          v-model.number="formData.importe"
          autocomplete="off"
          required
        />
        <field-messages name="importe" show="$dirty">
          <div slot="required" class="alert alert-dark mt-1">
            Campo requerido
          </div>
        </field-messages>
      </validate>

      <button
        class="btn btn-success my-3"
        :disabled="formState.$invalid"
        type="submit"
      >
        Enviar
      </button>
    </vue-form>

    <div v-if="importes.length" class="table-responsive">
      <table class="table table-dark">
        <tr>
          <th>Persona</th>
          <th>importe</th>
          <th>Descripcion</th>
        </tr>
        <tr v-for="(pers, i) in importes" :key="i">
          <td>{{ pers.nombre }}</td>
          <td>
            {{ pers.importe }}
          </td>
          <td>
            {{ pers.descripcion }}
          </td>
        </tr>
        <tr>
          Monto total
          <td v-if="acumimportes < 1000" class="alert alert-success">
            {{ acumimportes }}
          </td>
          <td
            v-else-if="acumimportes >= 1000 && acumimportes <= 5000"
            class="alert alert-danger"
          >
            {{ acumimportes }}
          </td>
          <td v-else>{{ acumimportes }}</td>
        </tr>
      </table>
    </div>
    <h3 class="alert alert-danger" v-else>¡No se encontraron importes!</h3>
  </div>
</template>

<script lang="js">

export default {
  name: 'src-components-formulario-importe',
  components: {},
  props: [],
  data() {
    return {
      formData: this.getFormInicial(),
      formState: {},
      nombreMin: 3,
      nombreMax: 15,
      importes: [],

      acumimportes: 0,
    }
  },
  computed: {},
  mounted() {},
  methods: {
    getFormInicial() {
      return {
        nombre: '',
        descripcion: '',
        importe: '',
      }
    },
    enviar() {
      let note = this.formData
      this.importes.push(note)
      this.acumimportes += note.importe
      this.formData = this.getFormInicial()
      this.formState._reset()
    }
  }
}
</script>

<style scoped lang="css">
.jumbotron {
  background-color: rgb(9, 9, 128);
  color: white;
}
hr {
  background-color: #eee;
}
</style>
