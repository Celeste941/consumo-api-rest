<template>
  <h1>¡BIENVENIDO A NUESTRO CHAT!</h1>
  <main v-if="user1 && user2">
    <section>
      <img :src="user1.picture.large" alt="">
      <p class="user1-chat">{{ user1.name.first }} {{ user1.name.last }}</p>

      <form @submit.prevent="enviarMensaje('user1')" action="">
        <input type="color" v-model="mensaje1.color">
        <textarea @keyup.enter="enviarMensaje('user1')" v-model="mensaje1.texto"></textarea>
        <button type="submit">Enviar</button>
      </form>

    </section>

    <ChatInteractivo :mensajes="mensajes" :user1="user1" :user2="user2" />

    <section>
      <img :src="user2.picture.large" alt="">
      <p>{{ user2.name.first }} {{ user2.name.last }}</p>

      <form @submit.prevent="enviarMensaje('user2')" action="">
        <input type="color" v-model="mensaje2.color">
        <textarea @keyup.enter="enviarMensaje('user2')" v-model="mensaje2.texto"></textarea>
        <button type="submit">Enviar</button>
      </form>

    </section>
  </main>
</template>

<script>
import axios from 'axios';
import ChatInteractivo from './components/ChatInteractivo.vue';

export default {
  name: 'App',
  components: {
    ChatInteractivo,
  },
  data() {
    return {
      user1: null,
      user2: null,
      mensaje1: { texto: '', color: '#000000' },
      mensaje2: { texto: '', color: '#000000' },
      mensajes: [],
    };
  },
  async created() {
    try {
      const response = await axios.get('https://randomuser.me/api?results=2');
      this.user1 = response.data.results[0];
      this.user2 = response.data.results[1];
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    enviarMensaje(user) {
      const mensaje = user === 'user1' ? this.mensaje1 : this.mensaje2;
      const propietario = user === 'user1' ? this.user1 : this.user2;

      this.mensajes.push({
        texto: mensaje.texto,
        color: mensaje.color,
        propietario: `${propietario.name.first} ${propietario.name.last}`,
        id: user
      });
      mensaje.texto = '';// Limpiamos el input "mensaje del usuario1-2"
    },
  },
};
</script>

<style>
body {
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
}

h1 {
  width: 30%;
  padding: 5px;
  margin: 0 auto;
  text-align: center;
  box-shadow: 5px 5px 5px;
  color: #887f7f;
}

main {
  display: flex;
  justify-content: center;
  gap: 10px;
  padding: 20px;
}

section {
  width: 200px;
  height: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 15px;
  gap: 5px;
  border: 2px solid #ddd;
  background-color: #eeeeee;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;

}

img {
  width: 100%;
  object-fit: cover;
}

p {
  padding: 0;
  margin: 0;
}

textarea {
  height: 100px;
  resize: none;
}

input {
  width: 100%;
  height: 30px;
}

button {
  background-color: white;
  border: 0.5px solid grey;
  cursor: pointer;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

button:hover {
  background-color: rgba(149, 230, 252, 0.286);
}
</style>
