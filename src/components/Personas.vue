<template>
  <div class="container-fluid mt-3">
    <div class="form-group">
      <input type="text" class="form-control" v-model="filtroNombre" placeholder="Filtrar por nombre y apellido" style="min-width: 300px;" />
    </div>
    <div class="form-group mt-2">
      <input type="text" class="form-control" v-model="filtroDni" placeholder="Filtrar por DNI" style="min-width: 300px;"/>
    </div>

    <!-- Alertas Bootstrap -->
    <div v-if="mostrarAlerta" class="alert alert-warning mt-2" role="alert">
      Debes ingresar al menos 3 caracteres en alguno de los filtros.
    </div>

    <div class="card-deck m-0 mt-4">
      <div class="row">
        <div class="col-md-4 mb-3" v-for="persona in personasFiltradas" :key="persona.dni">
          <div class="card">
            <div class="card-body">
              <h5 class="card-title">{{ getNombreCompleto(persona) }}</h5>
              <p class="card-text">DNI: {{ persona.dni }}</p>
              <a href="#" class="card-link">{{ persona.correo }}</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Personas",
  data() {
    return {
      filtroNombre: "",
      filtroDni: "",
      personas: [
        {
          nombre: "Daniel",
          apellido: "Sanchez",
          correo: "danielsanchez68@hotmail.com",
          dni: "20442873"
        },
        {
          nombre: "Juan",
          apellido: "Perez",
          correo: "j@p.gmail.com",
          dni: "12345678"
        },
        {
          nombre: "Ana",
          apellido: "Suarez",
          correo: "a@s.gmail.com",
          dni: "87654321"
        },
        {
          nombre: "Alejo",
          apellido: "Madama",
          correo: "alejomadama1@gmail.com",
          dni: "44049678"
        }
      ]
    };
  },
  computed: {
    personasFiltradas() {
      const nombreOk = this.filtroNombre.length >= 3;
      const dniOk = this.filtroDni.length >= 3;

      if (!nombreOk && !dniOk) return [];

      return this.personas.filter((p) => {
        const nombreCompleto = `${p.nombre} ${p.apellido}`.toLowerCase();
        const dni = p.dni.toLowerCase();

        const coincideNombre = nombreOk
          ? nombreCompleto.includes(this.filtroNombre.toLowerCase())
          : true;

        const coincideDni = dniOk
          ? dni.includes(this.filtroDni.toLowerCase())
          : true;

        return coincideNombre && coincideDni;
      });
    },
    mostrarAlerta() {
      return (
        (this.filtroNombre.length > 0 && this.filtroNombre.length < 3) ||
        (this.filtroDni.length > 0 && this.filtroDni.length < 3)
      );
    }
  },
  methods: {
    getNombreCompleto(p) {
      return `${p.nombre} ${p.apellido}`;
    }
  }
};
</script>

<style scoped>
input.form-control {
  font-size: 1.25rem;
  padding: 0.75rem 1rem;
}

.alert {
  font-size: 1.1rem;
}

.card {
  font-size: 1.25rem;
  padding: 1rem;
}

.card-title {
  font-size: 1.5rem;
  font-weight: bold;
}

.card-text,
.card-link {
  font-size: 1.2rem;
}

.container-fluid {
  max-width: 1000px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 1.5rem;
}

@media (min-width: 768px) {
  .col-md-4 {
    flex: 0 0 30%;
    max-width: 30%;
  }
}
</style>
