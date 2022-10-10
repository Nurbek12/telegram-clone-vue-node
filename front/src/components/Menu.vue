<template>
  <div class="menu-table" v-if="user._id === menumessage.sender._id"
    :style="`--x: ${menumessage.x}px; --y: ${menumessage.y}px;`">
      <div class="menu-close" @click="set_menu_mess(null)">
          <i class="bx bx-x"></i>
      </div>
      <div class="menu-item" @click="() => {changeEditMess(menumessage); set_menu_mess(null)}">
        <i class="bx bxs-pencil" style="margin-right:20px;"></i> Изменить
      </div>
      <div class="menu-item" @click="() => {deleteMessage(menumessage._id); set_menu_mess(null)}">
        <i class="bx bx-trash" style="margin-right: 20px;"></i> Удалит
      </div>
    </div>
</template>

<script>
import { mapActions, mapGetters, mapMutations } from "vuex";

export default {
    name: 'Message',
    methods: {
      ...mapActions(['deleteMessage']),
      ...mapMutations(['changeEditMess', "set_menu_mess"]),
    },
    computed: mapGetters(['menumessage', 'user']),
} 
</script>

<style>

</style>