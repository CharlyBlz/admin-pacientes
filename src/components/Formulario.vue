<script setup>
    import { ref, reactive, computed } from 'vue';
    import Alerta from './Alerta.vue';
    // Se opta usar REACTIVE en lugar de REF
    // Con REF se tendrían que utilizar 5 variables diferentes (nombre, propietario,email, alta, síntomas)
    // Con REACTIVE se agrupan los datos para que estén todos en una sola variable reactiva que será un objeto
    const alerta = reactive({
        tipo:'',
        mensaje:''
    })
    // Alerta no tiene relación con paciente, por lo que no se agrega en el state de paciente
    // El punto de reactive es agrupar datos relacionados
    
    /*
    const leerNombre = e => {
        nombre.value = e.target.value
    }
    */

   // SE DEFINEN EMITS 
   const emit = defineEmits (['update:nombre', 'update:propietario', 'update:email', 'update:alta', 'update:sintomas', 'guardar-paciente'])

   // SE DEFINEN PROPS
   const props = defineProps({
    id: {
        type: [String, null],
        required: true
    },
    nombre: {
        type: String,
        required: true
    },
    propietario: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    alta: {
        type: String,
        required: true
    },
    sintomas: {
        type: String,
        required: true
    },
   }) 

   const validar = () => {
    console.log()
    // if([paciente.nombre, paciente.propietario].includes(''))
    // Ya no se valida el objeto "PACIENTE" pues ahora se encuentra en App.vue
    if(Object.values(props).includes('')){
        alerta.mensaje = "Todos los campos son obligatorios"
        alerta.tipo = "error"
        return
    }
    // En caso de agregar info extra, ejemplo
    // emit('guardar-paciente', "exitoso")
    emit('guardar-paciente')
    alerta.mensaje = "Paciente almacenado correctamente"
    alerta.tipo = 'exito'
    setTimeout(() => {
       Object.assign(alerta, {
        tipo:'',
        mensaje:''
       }) 
    }, 3000)
   }

   // COMPUTED PROPERTIES
   const editando = computed(() => {
        return props.id
   }) 

</script>

<template>
    <!--Width del 50%, para tener 2 columnas con el mismo ancho de pantalla-->
    <div class="md:w-1/2">
        <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
        
        <p class="text-lg mt-5 text-center mb-10">
            Añade Pacientes y
            <span class="text-indigo-600 font-bold"> Adminístralos</span>
        </p>
        <!-- md: mediano; rounded: border radius -->
         <Alerta
            v-if = "alerta.mensaje"
            :alerta="alerta"
         />
        <form 
            class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
            @submit.prevent="validar"
        >
            <!--.prevent es un EVENT MODIFIER y previene la acción por default-->
            <!-- Este v-on (@) en específico se llamará "submit", por eso el uso de dos puntos (:) -->
            <div class="mb-5">
                <label 
                for="mascota"
                class="block text-gray-700 uppercase font-bold"
                >
                    Nombre mascota
                </label>
                <input 
                id="mascota"
                type="text"
                placeholder="Nombre de la mascota"
                class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                :value="nombre"
                @input="$emit('update:nombre', $event.target.value)"
                >
                <!--:VALUE="NOMBRE" es un atributo dinámico-->
                <!-- Con METHOD HANDLER -> @input="leerNombre" -->
                <!-- Con INLINE HANDLER -> @input="(e) => nombre = e.target.value" -->
                <!-- :value="" es un atributo dinámico -->
                <!-- nombre = e.target.value así se estará escribiendo el state de "nombre" con la info. que ingrese el usuario en el input -->
                <!-- Esto se conoce como TWO WAY DATA BINDING (compartir los datos de forma bidireccional) -->
            </div>
            <!--v-on:input="" o @input=""-->
            <div class="mb-5">
                <label 
                for="propietario"
                class="block text-gray-700 uppercase font-bold"
                >
                    Nombre Propietario
                </label>
                <input 
                id="propietario"
                type="text"
                placeholder="Nombre del propietario"
                class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                :value="propietario"
                @input="$emit('update:propietario', $event.target.value)"
                >
            </div>

            <div class="mb-5">
                <label 
                for="email"
                class="block text-gray-700 uppercase font-bold"
                >
                    Email
                </label>
                <input 
                id="email"
                type="email"
                placeholder="Email del propietario"
                class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                :value="email"
                @input="$emit('update:email', $event.target.value)"
                >
            </div>

            <div class="mb-5">
                <label 
                for="alta"
                class="block text-gray-700 uppercase font-bold"
                >
                    Alta
                </label>
                <input 
                id="alta"
                type="date"
                class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                :value="alta"
                @input="$emit('update:alta', $event.target.value)"
                >
            </div>

            <div class="mb-5">
                <label 
                for="sintomas"
                class="block text-gray-700 uppercase font-bold"
                >
                    Sintomas
                </label>
                <textarea 
                id="sintomas"
                type="text"
                placeholder="Describe los síntomas"
                class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                :value="sintomas"
                @input="$emit('update:sintomas', $event.target.value)"
                />
            </div>
            <!--DATE no requiere placeholder-->
            <input 
            type="submit"
            class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700 cursor-pointer transition-colors"
            :value="[editando ? 'Guardar cambios' : 'Registrar paciente']"
            >
        </form>
    </div>
</template>

