<template>
<main class="app">
  <h1>Tiger Woods Quiz</h1>
  <section class="quiz" v-if="!quizCompleted">
    <div class="quiz-info">
      <span class="question">{{ getCurrentQuestion.question }}</span>
    </div>
    <div class="options">
      <label 
        v-for="(option, index) in getCurrentQuestion.options" 
        :key="index"
        :class="`option 
                ${getCurrentQuestion.selected == index ? index == getCurrentQuestion.answer ? 'correct' : 'wrong' : ''} 
                ${getCurrentQuestion.selected != null && index != getCurrentQuestion.selected ? 'disabled' : ''}`">
        <input 
        @change="setAnswer"
        type="radio" 
        :name="getCurrentQuestion.index" 
        :value="index"
        v-model="getCurrentQuestion.selected"
        :disabled="getCurrentQuestion.selected">
        <span>{{ option }}</span>
      </label>
    </div>
    <button
    @click="nextQuestion"
    :disabled="!getCurrentQuestion.selected">
    {{getCurrentQuestion.index == questions.length -1 ? 'Finish' : getCurrentQuestion.selected == null ? 'Select an option' : 'Next question'}}
    </button>
    <span style="float:right;" class="score">{{ score }} / {{ questions.length }}</span>
  </section>
  <section v-else>
    <h2>You have finished the quiz!</h2>
    <p>Your score is {{ score }} / {{ questions.length }}</p>
  </section>
  <section class="additionalData"
            v-if="getCurrentQuestion.selected != null && getCurrentQuestion.selected != getCurrentQuestion.answer && !quizCompleted">
            <span>{{getCurrentQuestion.data}}</span>
  </section>
</main>
</template>

<script setup>
import { ref, computed } from 'vue';

const questions = ref([
  {
    question: "When did Tiger Woods last win a major?",
    answer: 0,
    options: [
      '2019','2017','2015'    
             ],
    selected: null,
    data: 'The 2019 Masters Championship'
  },
  {
    question: "What colour does Tiger Woods wear on a Sunday?",
    answer: 2,
    options: [
      'Blue','Green','Red'    
             ],
    selected: null,
    data: 'Tiger Woods wears red on Sundays. That\'s just what he does!'
  },
  {
    question: "What is Tiger Woods full name?",
    answer: 1,
    options: [
      'John','Eldrick','Jimmy'    
             ],
    selected: null,
    data: 'Tiger\'s full name is Eldrick Tont "Tiger" Woods'
  },
   {
    question: "When Tiger won the 2000 British Open, what course was it played on?",
    answer: 0,
    options: [
      'St Andrews','Royal Birkdale','The Belfry'    
             ],
    selected: null,
    data: 'He completed the Grand Slam at the Old Course St Andrews'
  },
  {
    question: "What other golfer besides Tiger finished under par in the 2000 U.S. Open?",
    answer: 2,
    options: [
      'Ernie Els','Jimenez','No one else did'    
             ],
    selected: null,
    data: 'Tiger was 12 under par for the tournament, while the next closest were Els and Jimenez at 3 over par.'
  },
  {
    question: "Who paid Tiger the most for endorsements?",
    answer: 1,
    options: [
      'Bridgestone','Nike','Taylormade'    
             ],
    selected: null,
    data: 'Nike signed Tiger to a 20 year contract.'
  },
  {
    question: "How old was Tiger when he first won the Masters?",
    answer: 2,
    options: [
      '20','19','21'    
             ],
    selected: null,
    data: 'At 21 he was the youngest player ever to have won'
  },
  {
    question: "What year did Tiger complete the career Grand Slam?",
    answer: 1,
    options: [
      '1999','2000','2002'    
             ],
    selected: null,
    data: 'In 2000 he become the fifth player in history to do this.'
  },
  {
    question: "When did Tiger win his first Green Jacket?",
    answer: 0,
    options: [
      '1997','2003','2005'    
             ],
    selected: null,
    data: 'He won his first in 1997. His second came four years later in 2001.'
  },
   {
    question: "How many golfers have won more majors than Tiger Woods?",
    answer: 1,
    options: [
      '0','1','2'    
             ],
    selected: null,
    data: 'Tiger Woods has won 15 major championships on the PGA Tour. There is only one person who has more than him - Jack Nicklaus'
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);

// Calculate Score
const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if(q.selected == q.answer){
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(()=> {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})

const setAnswer = e => {
  questions.value[currentQuestion.value].selected = e.target.value
  e.target.value = null
}

const nextQuestion = () => {
  if(currentQuestion.value < questions.value.length -1){
      currentQuestion.value++
  }
  else {
    quizCompleted.value = true;
  }
}
</script>
