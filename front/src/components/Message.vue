<template>
  <div :id="message._id" @click="changeMess($event, message)" :class="`message-wrapper ${message.sender === null? 'alert': 
    message.sender._id === user._id? 'me': 'out'}`">
    <div class="message-image"
      v-if="currentRoom.group &&message.sender !== null &&message.sender._id !== user._id">
      <div class="imgbox">
        <img :src="message.sender.avatar[message.sender.avatar.length - 1]"
          v-if="message.sender.avatar.length !== 0" alt="" />
        <div :class="`noimg ${message.sender.usercolor}`" v-else>
          {{ message.sender.name.at(0) }}
        </div>
      </div>
    </div>
    <div :class="`message ${message.sender === null? 'alert': message.sender._id === user._id? 'me': 'out'} ${message.image?'has-image':''}`">
      <span class="name"
        v-if="currentRoom.group &&message.sender !== null &&message.sender._id !== user._id"
        :style="{ color: message.sender.usercolor }">
        {{ message.sender.name }}
      </span>
      <img :src="message.image" v-if="message.image">
      <p>
        {{ message.text }}
        <span
          v-if="message.sender !== null"
          :style="`color: var(--${
            message.sender._id === user._id ? 'active-color' : 'lead-text'
          }); display:flex; align-items:flex-end;`">
          {{ message.date.time }}
          <i v-if="message.sender._id === user._id"
            :class="`check-icon bx bx-check${message.view ? '-double' : ''}`"></i>
        </span>
      </p>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapMutations } from "vuex";

export default {
    name: 'Message',
    props: ['message'],
    data: () => ({
      x: 0,
      y: 0,
    }),
    methods: {
      ...mapMutations(['set_menu_mess']),
      changeMess(e, mess){
        this.x = e.clientX;
        this.y = e.clientY;
        this.set_menu_mess({...mess, x: this.x, y: this.y});
      },
    },
    computed: mapGetters(["currentRoom", "user"]),
}
</script>

<style>

</style>