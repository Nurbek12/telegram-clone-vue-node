<template>
  <div class="message-gruop-data">
    <div class="message alert message-time">
      <p>
        {{ messages[0].date.day }}
      </p>
    </div>
    <transition-group name="message">
      <Message
        v-for="message in messages"
        :key="message._id" :message="message" />
    </transition-group>
  </div>
</template>

<script>
import Message from './Message.vue';

export default {
  name: "MessageDate",
  props: ["messages"],
  components: {
    Message,
  }
};
</script>

<style>
.message-move, /* apply transition to moving elements */
.message-enter-active,
.message-leave-active {
  transition: all 0.3s ease;
}
.message-leave-to {
  opacity: 0;
}

.message-enter-from{
  opacity: 0;
  transform: translateY(20px);
}

/* ensure leaving items are taken out of layout flow so that moving
   animations can be calculated correctly. */
.list-leave-active {
  position: absolute;
}
</style>

