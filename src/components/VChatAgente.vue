<script setup>
import { ref } from "vue"

const isOpen = ref(false)

const conversations = ref([
  { id: 1, name: "Juan Pérez" },
  { id: 2, name: "Ana García" },
  { id: 3, name: "Carlos López" }
])

const activeConversation = ref(conversations.value[0])

const messages = ref([
  { from: "client", text: "Hola necesito ayuda." },
  { from: "agent", text: "Hola 👋 con gusto te ayudo." }
])

const reply = ref("")

const toggleChat = () => {
  isOpen.value = !isOpen.value
}

const sendReply = () => {
  if (!reply.value) return

  messages.value.push({
    from: "agent",
    text: reply.value
  })

  reply.value = ""
}
</script>

<template>
  <div class="fixed bottom-6 right-6 z-50">

    <!-- BOTÓN -->
    <button
      v-if="!isOpen"
      @click="toggleChat"
      class="w-14 h-14 rounded-full bg-[#1152d4] text-white shadow-xl flex items-center justify-center hover:scale-110 transition"
    >
      <i class="pi pi-comments text-xl"></i>
    </button>

    <!-- PANEL -->
    <div
      v-if="isOpen"
      class="w-[420px] h-[500px] bg-white rounded-xl shadow-2xl flex flex-col overflow-hidden"
    >

      <!-- HEADER -->
      <div class="bg-[#1152d4] text-white p-3 flex justify-between items-center">
        <span class="font-bold">Panel de Soporte</span>

        <button @click="toggleChat">
          <i class="pi pi-times"></i>
        </button>
      </div>

      <!-- CONTENIDO -->
      <div class="flex flex-1">

        <!-- LISTA CLIENTES -->
        <div class="w-40 border-r overflow-y-auto">

          <div
            v-for="conv in conversations"
            :key="conv.id"
            @click="activeConversation = conv"
            class="p-3 cursor-pointer hover:bg-gray-100 text-sm"
          >
            {{ conv.name }}
          </div>

        </div>

        <!-- CHAT -->
        <div class="flex-1 flex flex-col">

          <!-- HEADER CLIENTE -->
          <div class="p-3 border-b bg-gray-100 text-sm font-semibold">
            Chat con {{ activeConversation.name }}
          </div>

          <!-- MENSAJES -->
          <div class="flex-1 p-3 overflow-y-auto flex flex-col gap-2 bg-gray-50">

            <div
              v-for="(msg,index) in messages"
              :key="index"
              :class="[
                'px-3 py-2 rounded-lg max-w-[70%] text-sm',
                msg.from === 'agent'
                  ? 'bg-[#1152d4] text-white self-end'
                  : 'bg-gray-200 text-gray-800 self-start'
              ]"
            >
              {{ msg.text }}
            </div>

          </div>

          <!-- INPUT -->
          <div class="p-2 border-t flex gap-2">

            <input
              v-model="reply"
              @keyup.enter="sendReply"
              placeholder="Responder..."
              class="flex-1 border rounded-lg px-3 py-1 text-sm"
            />

            <button
              @click="sendReply"
              class="bg-[#1152d4] text-white px-3 rounded-lg"
            >
              <i class="pi pi-send"></i>
            </button>

          </div>

        </div>

      </div>

    </div>

  </div>
</template>
