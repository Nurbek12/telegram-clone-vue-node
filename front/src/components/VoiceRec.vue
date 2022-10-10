<template>
  <div class="backgr" @click="set_voicerecmodal(false)"></div>
  <div class="modal center-mic">
      <div class="mic">
        <div class="animt-mic1" v-if="recognition"></div>
        <div class="animt-mic2" v-if="recognition"></div>
        <div class="mic-btn" v-if="!recognition" @click="soundToText()">
            <i class="bx bxs-microphone"></i>
        </div>
        <div class="mic-btn animat" v-if="recognition" @click="stopVoice()">
            <i class="bx bx-pause"></i>
        </div>
      </div>
    <input type="text" v-model="text" @keydown.enter="speak">
  </div>
</template>

<script>
import { mapMutations } from "vuex";

export default {
  name: "VoiceRec",
  data: () => ({
    lang: "ru-Ru",
    runtimeTranscription_: "",
    transcription_: [],
    recognition: null,
    text: '',
  }),
  methods: {
    ...mapMutations(["set_voicerecmodal"]),
    soundToText() {
      console.log("start voice");
      window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      this.recognition = new window.SpeechRecognition();
      this.recognition.lang = this.lang;
      this.recognition.interimResults = true;
      this.recognition.addEventListener("result", (event) => {
        const text = Array.from(event.results)
          .map((result) => result[0])
          .map((result) => result.transcript)
          .join("");
        this.runtimeTranscription_ = text;
      });

    //   this.recognition.addEventListener("end", () => {
    //     this.transcription_.push(this.runtimeTranscription_);
    //     this.runtimeTranscription_ = "";
    //     this.recognition.stop();
    //     this.recognition = null;
    //   });
      this.recognition.start();
    },
    stopVoice() {
        this.recognition.stop();
        this.recognition = null;
        console.log("stop voice");
    },
    speak(){
        try{
            const utter = new SpeechSynthesisUtterance(this.text);
            // utter.rate = 1;
            // utter.volume = 1;
            speechSynthesis.speak(utter);
        }catch(err){
            console.log(err);
        }
    }
  },
};
</script>

<style>
.mic{
    display: flex;
    position: relative;
    justify-content: center;
    align-items: center;
}
.center-mic{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    z-index: 9399493;
}
.mic-btn{
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background: linear-gradient(30deg, rgb(204, 0, 204), rgb(85, 12, 255));
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    color: #fff;
    position: relative;
    box-shadow: inset 4px 4px 10px rgba(255,255,255,0.5), inset -4px -4px 10px rgba(90, 90, 90, 0.5);
}
.mic-btn.animat{
    animation: sound 1s linear infinite;
}
.animt-mic2{
    position: absolute;
    content: '';
    width: 120px;
    height: 130px;
    border-radius: 45%;
    background: rgba(167, 167, 167, 0.3);
    animation-delay: .5s;
    animation: 4s loader linear infinite;
}
.animt-mic1{
    position: absolute;
    content: '';
    width: 117px;
    height: 125px;
    border-radius: 50%;
    background: rgba(201, 200, 200, 0.3);
    animation-delay: -.1s;
    animation: 3s loader linear infinite;
}
@keyframes sound{
    0%, 100%{
        filter: hue-rotate(10deg);
    }
    50%{
        filter: hue-rotate(30deg);
    }
}
</style>