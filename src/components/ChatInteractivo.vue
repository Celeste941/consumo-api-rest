<template>
  <div class="container">
    <div v-for="(mensaje, index) in mensajes" :key="index"
      :class="['mensaje', { 'mensaje-propio': mensaje.id === 'user2' }]">
      <span>{{ mensaje.propietario }}</span>
      <p :style="{ backgroundColor: mensaje.color }">{{ mensaje.texto }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ChatInteractivo",
  props: {
    mensajes: Array,
    user1: Object,
    user2: Object
  },
};
</script>

<style scoped>
.container {
  width: 400px;
  height: 400px;
  padding: 15px;
  border: 2px solid #8b8b8ba0;
  font-size: 16px;
  line-height: 1.5;
  overflow-y: auto;
  background-color: #eeeeee;
  box-shadow: 0 0 8px #8b8b8ba0;
}

.mensaje {
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
  color: #fff;
}

.mensaje-propio {
  text-align: right;
}

p {
  padding: 3px 5px;
  border-radius: 20px;
  overflow: hidden;
  word-wrap: break-word;
  word-break: break-word;
  white-space: pre-wrap;
}

span {
  color: #676460cb;
}
</style>
