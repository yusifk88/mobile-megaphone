<template>
  <div id="app">
    <div :class="['min-h-screen flex flex-col font-sans select-none transition-colors duration-300', themeClasses.bg]">
      <!-- --- Header --- -->
      <header class="px-6 pt-8 pb-4 flex justify-between items-center shrink-0 mt-10">
        <div>
          <h1 class="text-2xl font-bold  text-purple-500  bg-clip-text ">
            Mobile Megaphone
          </h1>
        </div>
        <div class="flex gap-2">
          <button @click="toggleTheme"
                  :class="['p-2 rounded-full border transition-all active:scale-90', themeClasses.iconBtn]">
            <span v-if="theme === 'dark'" class="text-amber-400">☀️</span>
            <span v-else class="text-slate-600">🌙</span>
          </button>
<!--          <button :class="['p-2 rounded-full border', themeClasses.iconBtn]">-->
<!--            <svg class="w-5 h-5 opacity-50" fill="none" stroke="currentColor" viewBox="0 0 24 24">-->
<!--              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"-->
<!--                    d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"></path>-->
<!--              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"-->
<!--                    d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path>-->
<!--            </svg>-->
<!--          </button>-->
        </div>
      </header>

      <!-- --- Main Visualizer Area --- -->
      <main class="flex-1 px-6 flex flex-col gap-6 overflow-y-auto pb-40 no-scrollbar">
        <!-- Clickable Visualizer Container -->
        <div

            :class="['relative aspect-square w-full max-w-sm mx-auto bg-gradient-to-br rounded-3xl border flex flex-col items-center justify-center overflow-hidden transition-all duration-500 shrink-0 cursor-pointer active:scale-[0.98]', themeClasses.visualizer]"
        >
          <div
              :class="['absolute inset-0 bg-purple-500/10 animate-pulse transition-opacity duration-500', isMicOn ? 'opacity-100' : 'opacity-0']"></div>

          <div @click="toggleMic"
               :class="['transition-all duration-500 transform', isMicOn ? 'scale-110' : 'scale-100']">
            <div
                :class="['p-8 rounded-full transition-all', isMicOn ? 'bg-purple-600 shadow-[0_0_40px_rgba(147,51,234,0.4)] text-white' : (theme === 'dark' ? 'bg-slate-700 text-slate-400' : 'bg-slate-200 text-slate-400')]">
              <svg v-if="isMicOn" class="w-12 h-12" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z"></path>
              </svg>

              <svg v-else class="w-12 h-12" xmlns="http://www.w3.org/2000/svg" width="16" height="16"
                   fill="currentColor" viewBox="0 0 16 16">
                <path
                    d="M13 8c0 .564-.094 1.107-.266 1.613l-.814-.814A4 4 0 0 0 12 8V7a.5.5 0 0 1 1 0zm-5 4c.818 0 1.578-.245 2.212-.667l.718.719a5 5 0 0 1-2.43.923V15h3a.5.5 0 0 1 0 1h-7a.5.5 0 0 1 0-1h3v-2.025A5 5 0 0 1 3 8V7a.5.5 0 0 1 1 0v1a4 4 0 0 0 4 4m3-9v4.879l-1-1V3a2 2 0 0 0-3.997-.118l-.845-.845A3.001 3.001 0 0 1 11 3"/>
                <path
                    d="m9.486 10.607-.748-.748A2 2 0 0 1 6 8v-.878l-1-1V8a3 3 0 0 0 4.486 2.607m-7.84-9.253 12 12 .708-.708-12-12z"/>
              </svg>

            </div>
          </div>

          <div class="mt-8 text-center z-10">
            <h2 class="text-xl font-semibold" style="transition: 0.3s ease-in-out">{{ isMicOn ? "Speak" : 'Tap the mic to speak' }}</h2>
          </div>

          <!-- Animated Waveform -->
          <div class="absolute bottom-0 w-full h-24 flex items-end justify-center gap-1 pb-4 opacity-30 mt--10">
            <div
                v-for="i in 20"
                :key="i"
                class="w-1 bg-purple-500 rounded-full"
                :style="{
                                height: isMicOn || isPlaying ? (Math.random() * 80 + 20) + '%' : '10%',
                                transition: 'height 0.2s ease-in-out'
                            }"
            ></div>
          </div>

          <div class="absolute top-4 right-4 opacity-20">
            <div class="w-1.5 h-1.5 rounded-full bg-current animate-ping"></div>
          </div>
        </div>

        <!-- --- Track Selector --- -->
<!--        <button-->
<!--            @click="showTrackPicker = true"-->
<!--            :class="['w-full py-4 px-5 border rounded-2xl flex items-center gap-4 active:scale-95 transition-all shrink-0', themeClasses.card]"-->
<!--        >-->
<!--          <div class="p-2 bg-pink-500/10 rounded-lg">-->
<!--            <svg class="w-5 h-5 text-pink-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">-->
<!--              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"-->
<!--                    d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3"></path>-->
<!--            </svg>-->
<!--          </div>-->
<!--          <div class="flex-1 text-left">-->
<!--            <span :class="['block text-[10px] uppercase font-bold tracking-wider', themeClasses.subtext]">Background Music</span>-->
<!--            <span class="text-sm font-medium">{{-->
<!--                selectedTrack ? `${selectedTrack.title} - ${selectedTrack.artist}` : 'Browse your library'-->
<!--              }}</span>-->
<!--          </div>-->
<!--          <svg class="w-5 h-5 opacity-30" fill="none" stroke="currentColor" viewBox="0 0 24 24">-->
<!--            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>-->
<!--          </svg>-->
<!--        </button>-->

        <!-- --- Controls & Effects --- -->
        <div class="grid grid-cols-1 gap-6">
          <div class="space-y-4">

          </div>



          <div :class="['p-5 rounded-3xl border space-y-6 transition-all', themeClasses.panel]">
            <div class=" justify-between items-center">
            <p class="text-red-500 block">WARNING</p>
              <p>DO NOT TURN ON THE MICROPHONE WITHOUT HEADPHONES OR A WIRELESS/CABLE SPEAKER CONNECTED TO THE PHONE.</p>

              <p style="margin-top: 20px">This is a hubby project by
                <a href="https://linkedin.com/in/katulie" class="text-blue-200">Yussif Katulie</a>, Feel free to clone and modify it for your own use <a class="text-blue-200" href="https://github.com/yusifk88/mobile-megaphone">here</a>.
              </p>
            </div>
          </div>

        </div>
      </main>

      <!-- --- Playback Bar --- -->
<!--      <footer-->
<!--          :class="['fixed bottom-0 left-0 right-0 p-6 pt-10 bg-gradient-to-t to-transparent z-50 pointer-events-none', theme === 'dark' ? 'from-slate-950 via-slate-950 60%' : 'from-slate-50 via-slate-50 60%']">-->
<!--        <div-->
<!--            :class="['backdrop-blur-xl border rounded-full p-2 flex items-center gap-2 pointer-events-auto', theme === 'dark' ? 'bg-white/5 border-white/10' : 'bg-white/70 border-slate-200 shadow-lg']">-->
<!--          <button-->
<!--              @click="toggleMic"-->
<!--              :class="['w-12 h-12 rounded-full flex items-center justify-center transition-all', isMicOn ? 'bg-purple-600 text-white shadow-lg shadow-purple-500/30' : (theme === 'dark' ? 'bg-slate-800 text-slate-500' : 'bg-slate-200 text-slate-500')]"-->
<!--          >-->
<!--            <svg v-if="isMicOn" class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">-->
<!--              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"-->
<!--                    d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z"></path>-->
<!--            </svg>-->
<!--            <svg class="w-5 h-5" v-else xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"-->
<!--                 viewBox="0 0 16 16">-->
<!--              <path-->
<!--                  d="M13 8c0 .564-.094 1.107-.266 1.613l-.814-.814A4 4 0 0 0 12 8V7a.5.5 0 0 1 1 0zm-5 4c.818 0 1.578-.245 2.212-.667l.718.719a5 5 0 0 1-2.43.923V15h3a.5.5 0 0 1 0 1h-7a.5.5 0 0 1 0-1h3v-2.025A5 5 0 0 1 3 8V7a.5.5 0 0 1 1 0v1a4 4 0 0 0 4 4m3-9v4.879l-1-1V3a2 2 0 0 0-3.997-.118l-.845-.845A3.001 3.001 0 0 1 11 3"/>-->
<!--              <path-->
<!--                  d="m9.486 10.607-.748-.748A2 2 0 0 1 6 8v-.878l-1-1V8a3 3 0 0 0 4.486 2.607m-7.84-9.253 12 12 .708-.708-12-12z"/>-->
<!--            </svg>-->
<!--          </button>-->

<!--          <div class="flex-1 flex items-center justify-center gap-6">-->
<!--            <button :class="[themeClasses.subtext, 'hover:text-pink-500 transition-colors']">-->
<!--              <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">-->
<!--                <path d="M6 6h2v12H6zm3.5 6l8.5 6V6z"></path>-->
<!--              </svg>-->
<!--            </button>-->
<!--            <button-->
<!--                @click="isPlaying = !isPlaying"-->
<!--                :class="['w-14 h-14 rounded-full flex items-center justify-center shadow-lg active:scale-95 transition-transform', theme === 'dark' ? 'bg-white text-slate-950' : 'bg-slate-900 text-white']"-->
<!--            >-->
<!--              <svg v-if="isPlaying" class="w-7 h-7" fill="currentColor" viewBox="0 0 24 24">-->
<!--                <path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"></path>-->
<!--              </svg>-->
<!--              <svg v-else class="w-7 h-7 ml-1" fill="currentColor" viewBox="0 0 24 24">-->
<!--                <path d="M8 5v14l11-7z"></path>-->
<!--              </svg>-->
<!--            </button>-->
<!--            <button :class="[themeClasses.subtext, 'hover:text-pink-500 transition-colors']">-->
<!--              <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">-->
<!--                <path d="M6 18l8.5-6L6 6v12zM16 6v12h2V6h-2z"></path>-->
<!--              </svg>-->
<!--            </button>-->
<!--          </div>-->

<!--          <div class="w-12 h-12 flex items-center justify-center">-->
<!--            <div :class="['w-1 h-6 rounded-full relative overflow-hidden', themeClasses.sliderBg]">-->
<!--              <div class="absolute bottom-0 w-full bg-pink-500 transition-all duration-300"-->
<!--                   :style="{ height: isPlaying ? '60%' : '0%' }"></div>-->
<!--            </div>-->
<!--          </div>-->
<!--        </div>-->
<!--      </footer>-->

      <!-- --- Track Picker --- -->
      <transition enter-active-class="animate__animated animate__slideInUp"
                  leave-active-class="animate__animated animate__slideOutDown">
        <div v-if="showTrackPicker" class="fixed inset-0 z-[60] flex items-end justify-center">
          <div class="absolute inset-0 bg-black/60 backdrop-blur-sm" @click="showTrackPicker = false"></div>
          <div :class="['relative w-full max-w-lg rounded-t-[40px] p-8 shadow-2xl border-t', themeClasses.modal]">
            <div class="flex justify-between items-center mb-6">
              <h3 class="text-xl font-bold">Select Track</h3>
              <button @click="showTrackPicker = false" :class="['p-2 rounded-full', themeClasses.iconBtn]">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                </svg>
              </button>
            </div>

            <div class="space-y-3 max-h-[50vh] overflow-y-auto pr-2 no-scrollbar">
              <button
                  v-for="track in tracks"
                  :key="track.id"
                  @click="selectTrack(track)"
                  :class="['w-full p-4 rounded-2xl flex items-center gap-4 transition-all border', selectedTrack?.id === track.id ? 'bg-pink-500/10 border-pink-500' : 'border-transparent ' + (theme === 'dark' ? 'bg-slate-800/50' : 'bg-slate-100')]"
              >
                <div
                    :class="['p-3 rounded-xl', selectedTrack?.id === track.id ? 'bg-pink-500 text-white' : (theme === 'dark' ? 'bg-slate-700' : 'bg-white shadow-sm')]">
                  <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                          d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3"></path>
                  </svg>
                </div>
                <div class="flex-1 text-left">
                  <p class="font-semibold text-sm">{{ track.title }}</p>
                  <p :class="['text-xs', themeClasses.subtext]">{{ track.artist }}</p>
                </div>
                <span :class="['text-xs font-mono', themeClasses.subtext]">{{ track.duration }}</span>
              </button>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import {computed, ref} from "vue";
import {AudioLoop} from "audioloop";

export default {

  setup() {
    const theme = ref('dark');
    const isMicOn = ref(false);
    const isPlaying = ref(false);
    const selectedTrack = ref(null);
    const micVolume = ref(1);
    const musicVolume = ref(1);
    const echo = ref(0.3);
    const pitch = ref(1);
    const isAutoTuneOn = ref(false);
    const selectedKey = ref('C');
    const showTrackPicker = ref(false);

    const changeEffect = async () => {

      const options = {
        pitch: pitch.value,
        echo: echo.value,
        gain: micVolume.value,
        autotune: false
      };
      // await AudioLoop.updateEffects(options);

      await AudioLoop.setPitch(pitch.value);


    }



    const startKaraoke = async () => {

      try {
        // 1. Check permissions first
        const permStatus = await AudioLoop.checkPermissions();

        // Use optional chaining (?.) to prevent the crash if it's undefined
        if (permStatus?.microphone !== 'granted') {
          console.log("Requesting permission...");
          const request = await AudioLoop.requestPermissions({permissions: ['microphone']});

          if (request?.microphone !== 'granted') {
            alert("We need mic access to make the karaoke work!");
            return;
          }
        }

        // 2. Start the engine
        await AudioLoop.startStreaming();
        console.log("Audio Engine Started!");

      } catch (e) {
        console.error("Failed to start audio engine", e);
      }
    }

    const tracks = [
      {id: 1, title: "Bohemian Rhapsody", artist: "Queen", duration: "5:55"},
      {id: 2, title: "Stay", artist: "The Kid LAROI", duration: "2:21"},
      {id: 3, title: "Levitating", artist: "Dua Lipa", duration: "3:23"},
      {id: 4, title: "Rolling in the Deep", artist: "Adele", duration: "3:48"},
    ];

    const pianoKeys = [
      {note: 'C', type: 'white'},
      {note: 'C#', type: 'black'},
      {note: 'D', type: 'white'},
      {note: 'D#', type: 'black'},
      {note: 'E', type: 'white'},
      {note: 'F', type: 'white'},
      {note: 'F#', type: 'black'},
      {note: 'G', type: 'white'},
      {note: 'G#', type: 'black'},
      {note: 'A', type: 'white'},
      {note: 'A#', type: 'black'},
      {note: 'B', type: 'white'},
      {note: 'C ', type: 'white'},
    ];

    const themeClasses = computed(() => ({
      bg: theme.value === 'dark' ? 'bg-slate-950 text-slate-100' : 'bg-slate-50 text-slate-900',
      card: theme.value === 'dark' ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200 shadow-sm',
      panel: theme.value === 'dark' ? 'bg-slate-900/50 border-slate-800' : 'bg-slate-100/80 border-slate-200',
      subtext: theme.value === 'dark' ? 'text-slate-400' : 'text-slate-500',
      sliderBg: theme.value === 'dark' ? 'bg-slate-800' : 'bg-slate-200',
      iconBtn: theme.value === 'dark' ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200',
      modal: theme.value === 'dark' ? 'bg-slate-900 border-slate-800' : 'bg-white border-slate-200',
      visualizer: theme.value === 'dark' ? 'from-slate-900 to-slate-800 border-slate-700/50 shadow-2xl' : 'from-white to-slate-50 border-slate-200 shadow-xl'
    }));

    const toggleTheme = () => theme.value = theme.value === 'dark' ? 'light' : 'dark';
    const toggleMic = () => {
      if (isMicOn.value) {

        AudioLoop.stopStreaming();

      } else {
        startKaraoke()
      }
      isMicOn.value = !isMicOn.value;
    }
    const selectTrack = (track) => {
      selectedTrack.value = track;
      showTrackPicker.value = false;
      isPlaying.value = true;
    };

    return {
      theme, isMicOn, isPlaying, selectedTrack, micVolume, musicVolume,
      echo, pitch, isAutoTuneOn, selectedKey, showTrackPicker,
      tracks, pianoKeys, themeClasses,
      toggleTheme, toggleMic, selectTrack, startKaraoke, changeEffect
    };
  }
}

</script>


<style scoped></style>
