<template>
  <div class="container-fluid mt-3">
    <input
      type="text"
      class="form-control mb-2"
      v-model="filtroNombre"
      placeholder="Buscar por nombre o apellido..."
    />
    <input
      type="text"
      class="form-control mb-2"
      v-model="filtroDni"
      placeholder="Buscar por DNI..."
      inputmode="numeric"
    />

    <div v-if="mostrarAdvertencia" class="alert alert-warning mt-2">
      Ingrese al menos 3 caracteres en el/los filtro(s) que esté usando.
    </div>

    <div class="card-deck m-0">
      <div class="row">
        <div class="col" v-for="persona in personasFiltradas" :key="persona.dni">
          <div class="card mb-3">
            <div class="card-body">
              <h5 class="card-title">{{ getNombreCompleto(persona) }}</h5>
              <p class="card-text">DNI: {{ persona.dni }}</p>
              <a :href="'mailto:'+persona.correo" class="card-link">{{ persona.correo }}</a>
            </div>
          </div>
        </div>

        <div v-if="personasFiltradas.length === 0" class="col-12 text-center text-muted py-4">
          No hay resultados
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      filtroNombre: "",
      filtroDni: "",
      MIN: 3,
      personas: [
        { nombre: "Daniel", apellido: "Sanchez", correo: "daniel@hotmail.com", dni: "20442873" },
        { nombre: "Juan", apellido: "Pappalardo", correo: "juanpappal@gmail.com", dni: "34585060" },
        { nombre: "Ana", apellido: "Suarez", correo: "ana@gmail.com", dni: "87654321" },
      ],
    };
  },
  computed: {
    personasFiltradas() {
      return this.personas.filter((p) => {
        let coincideNombre = true;
        let coincideDni = true;

        if (this.filtroNombre.trim().length >= this.MIN) {
          const completo = `${p.nombre} ${p.apellido}`.toLowerCase();
          coincideNombre = completo.includes(this.filtroNombre.trim().toLowerCase());
        }
        if (this.filtroDni.trim().length >= this.MIN) {
          coincideDni = p.dni.includes(this.filtroDni.trim());
        }
        // AND si ambos filtros están activos; si alguno no llega a 3, no se aplica ese filtro
        return coincideNombre && coincideDni;
      });
    },
    mostrarAdvertencia() {
      const n = this.filtroNombre.trim().length;
      const d = this.filtroDni.trim().length;
      return (n > 0 && n < this.MIN) || (d > 0 && d < this.MIN);
    },
  },
  methods: {
    getNombreCompleto(persona) {
      return `${persona.nombre} ${persona.apellido}`;
    },
  },
};
</script>

<style scoped>
/* opcional */
</style>

