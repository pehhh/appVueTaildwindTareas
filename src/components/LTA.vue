<template>
  <div class="flex justify-center items-center min-h-screen">
    <div class="container mx-auto">
      <h1 class="text-center m-5 text-5xl font-extrabold">
        Lista de tareas pendientes
      </h1>
      <!-- inputs boton -->
      <div class="flex justify-center items-center">
        <input
          type="text"
          name="nombre"
          placeholder="Nueva Tarea"
          class="
            border-2 border-gray-400
            w-2/5
            mt-3
            py-2
            px-4
            text-gray-700
            focus:outline-none focus:bg-white focus:border-green-400
            text-md
          "
          v-model="tareaNueva"
        />
        <input
          type="button"
          value="Guardar"
          class="
            bg-blue-400
            w-40
            border-blue-400
            ml-3
            text-md
            hover:bg-green-400 hover:border-green-400
            text-white
            border-2
            mt-3
          "
          v-on:click="nuevaTarea"
        />
      </div>

      <!-- <p>
        <label for="nombre">Descripcion</label> <br />
        <input type="text" name="descripcion" />
      </p> -->
      <div class="flex justify-center items-center">
        <table class="table-auto">
          <thead>
            <tr>
              <th class="px-4 py-2">Tarea</th>
              <th class="px-4 py-2">#</th>
              <th class="px-4 py-2">#</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(tarea, index) in tareas" :key="index">
              <td class="border px-4 py-2">{{ tarea.tarea }}</td>

              <td v-on:click="editarTarea(index)"><span class="fa fa-pen text-center"></span></td>
              <td v-on:click="borrarTarea(index)"><span class="fa fa-trash text-center"></span></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "LTA",
  data() {
    return {
      tareaNueva: "",
      tareaEditada: null,
      tareas: [
        {
          tarea: "Cocinar Paella valenciana",
        },
        {
          tarea: "Cortar cebolla a taquitos",
        },
      ],
    };
  },
  methods: {
    nuevaTarea() {
      // si no hay nada en la tarea, salimos de la funcion con un return
      if (this.tareaNueva.length == 0) {
        return;
      }
      // si no es una tarea editada, sino insertada desde cero
      if(this.tareaEditada===null){
        this.tareas.push({
        tarea: this.tareaNueva,
      });
      }else{
        // sustituye la tarea antigua por la editada
        this.tareas[this.tareaEditada].tarea = this.tareaNueva
        this.tareaEditada=null
      }
      this.tareaNueva=''
      this.guardarTareas()
      
    },
    borrarTarea(indice) {
      this.tareas.splice(indice,1)
    },
    // hay que diferenciar si es una tarea nueva o editada orque va la campo input
    editarTarea(indice){
      // cuando yo pincho, la tarea va alinput para poder editarla
      this.tareaNueva=this.tareas[indice].tarea
      this.tareaEditada=indice
    },
     guardarTareas(){
    const parsed=JSON.stringify(this.tareas)
    localStorage.setItem('tareas',parsed)
  }
  },
  //algo que se ejecuta mientras que la app este en uso
  mounted(){
    if(localStorage.getItem('tareas')){
      try{
        this.tareas = JSON.parse(localStorage.getItem('tareas'))
      }catch(error){
        localStorage.removeItem('tareas')
      }
    }
  }
};
</script>


<style scoped>
</style>
