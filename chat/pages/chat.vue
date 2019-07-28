<template>
  <div class="c-wrap">
    <div class="c-chat" ref="block">
      <c-message
        v-for="(m, i) in messages"
        :key="i"
        :name="m.name"
        :text="m.text"
        :owner="m.id === user.id"
      ></c-message>
    </div>
    <div class="clr"></div>
    <div class="c-form">
      <c-chat-form />
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import Message from "@/components/message";
import ChatForm from "@/components/form";
export default {
  middleware: ["chat"],
  head() {
    return {
      title: `Комната ${this.user.room}`
    };
  },

  components: { "c-message": Message, "c-chat-form": ChatForm },
  computed: mapState(["user", "messages"]),
  watch: {
    messages() {
      setTimeout(() => {
        this.$refs.block.scrollTop = this.$refs.block.scrollHeight;
      });
    }
  }
};
</script>

<style scoped>
.c-wrap {
  height: 100%;
  position: relative;
  overflow: hidden;
}
.c-form {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 1rem;
  height: 80px;
  background: white;
}
.c-chat {
  position: absolute;
  top: 0;
  right: 0;
  left: 0;
  bottom: 80px;
  padding: 1rem;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}
</style>


