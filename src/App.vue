<script setup>
import { ref, onMounted } from 'vue';

let confettiInstance = null;

const participants = ref([]); 
const newParticipant = ref(''); 
const WinnerNum = ref(null);
const showParticipants = ref(false); 


const addParticipant = () => {
  if (newParticipant.value.trim()) {
    participants.value.push(newParticipant.value.trim());
    newParticipant.value = '';
  }
};


const removeParticipant = (index) => {
  participants.value.splice(index, 1);
  if (WinnerNum.value && !participants.value.includes(WinnerNum.value)) {
    WinnerNum.value = null;
  }
};


const editParticipant = (index, newName) => {
  if (newName.trim()) {
    participants.value[index] = newName.trim();
  }
};


const select = async () => {
  if (participants.value.length === 0) {
    alert('Please add at least one participant!');
    return;
  }
  
  const randomIndex = Math.floor(Math.random() * participants.value.length);
  WinnerNum.value = participants.value[randomIndex];
  await triggerConfetti();
};


const triggerConfetti = async () => {
  if (typeof window !== 'undefined') {
    if (!confettiInstance) {
      const confettiModule = await import('https://cdn.skypack.dev/canvas-confetti@1.9.2');
      confettiInstance = confettiModule.default;
    }
    
    confettiInstance({
      particleCount: 150,
      spread: 200,
      origin: { y: 0.6 },
      colors: ['#FFD700', '#FFA500', '#FF6347', '#32CD32', '#1E90FF', '#8A2BE2'],
      zIndex: 10000
    });
  }
};


const handleKeydown = (e) => {
  if (e.key === 'Enter') addParticipant();
};
</script>

<template>
  <section class="h-screen w-screen cu flex flex-col justify-start items-center p-4 overflow-y-auto">
  
    <h1 class="text-white text-2xl md:text-3xl text-center px-2 flex items-center flex-wrap justify-center gap-3 mb-6 mt-4">
      <img 
        src="/Muslims Deed.jpg" 
        alt="Muslims Deed" 
        class="h-14 w-14 md:h-16 md:w-16 rounded-full shadow-lg border-2 border-amber-300"
      />
      Muslims Deed & Proyas - Quiz Winner Selection
      <img 
        src="/Proyas Prokashon-03.jpg" 
        alt="Proyas Prokashon" 
        class="h-14 w-14 md:h-16 md:w-16 rounded-full shadow-lg border-2 border-amber-300"
      />
    </h1>

 
    <div class="w-full max-w-md mb-6">
      <div class="flex gap-2">
        <input
          v-model="newParticipant"
          @keydown="handleKeydown"
          type="text"
          class="flex-1 border border-amber-300 h-12 rounded-lg text-white pl-4 bg-black/20 placeholder:text-white/50"
          placeholder="Enter participant name"
        />
        <button
          @click="addParticipant"
          class="px-4 bg-amber-500 hover:bg-amber-600 text-white rounded-lg font-medium transition"
        >
          Add
        </button>
      </div>
    </div>

    
    <button
      @click="showParticipants = !showParticipants"
      class="mb-6 text-amber-300 hover:text-amber-200 text-sm underline"
    >
      {{ showParticipants ? 'Hide Participants' : 'View/Edit Participants' }}
    </button>

   
    <div v-if="showParticipants" class="w-full max-w-md mb-6 bg-black/20 rounded-xl p-4 border border-amber-300/30">
      <h3 class="text-white font-bold mb-3 text-center">Participants ({{ participants.length }})</h3>
      <div v-if="participants.length === 0" class="text-white/70 text-center py-2">
        No participants added yet
      </div>
      <div v-else class="space-y-2 max-h-60 overflow-y-auto">
        <div 
          v-for="(name, index) in participants" 
          :key="index"
          class="flex items-center gap-2 bg-white/10 p-2 rounded-lg"
        >
          <input
            :value="name"
            @change="(e) => editParticipant(index, e.target.value)"
            type="text"
            class="flex-1 bg-transparent text-white border-b border-amber-300/50 focus:outline-none focus:border-amber-300"
          />
          <button
            @click="removeParticipant(index)"
            class="text-red-400 hover:text-red-300 text-lg"
          >
            ×
          </button>
        </div>
      </div>
    </div>


    <button 
      @click="select" 
      class="mt-2 md:mt-4 text-white text-xl font-bold border-2 border-amber-300 px-8 py-4 rounded-full hover:bg-amber-300 hover:text-purple-900 transition-all duration-300 transform hover:scale-105 shadow-xl active:scale-95"
    >
      🎉 Select Winner 🎉
    </button>
    
  
    <div v-if="WinnerNum" class="mt-8 text-center">
      <h2 class="text-4xl md:text-6xl font-extrabold text-transparent bg-clip-text bg-linear-to-r from-amber-300 via-yellow-400 to-amber-300 animate-pulse">
        {{ WinnerNum }}
      </h2>
      <p class="text-white/90 mt-4 text-lg md:text-xl font-medium">
        🎊 Congratulations! You are the winner! 🎊
      </p>
    </div>
  </section>
</template>

<style scoped>
.cu {
  background-image: linear-gradient(177.6deg, rgba(20,0,113,1) 15.3%, rgba(1,0,62,1) 91.3%);
}


#confetti-canvas {
  position: fixed !important;
  top: 0 !important;
  left: 0 !important;
  width: 100% !important;
  height: 100% !important;
  pointer-events: none !important;
  z-index: 10000 !important;
}


::-webkit-scrollbar {
  width: 6px;
}
::-webkit-scrollbar-track {
  background: rgba(0,0,0,0.1);
}
::-webkit-scrollbar-thumb {
  background: rgba(255,215,0,0.3);
  border-radius: 3px;
}
</style>