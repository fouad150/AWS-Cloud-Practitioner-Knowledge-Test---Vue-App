<script setup>
import { ref, computed } from 'vue'
const questions_array=[

        {question: "Why is AWS more economical than traditional data centers for applications with varying compute workloads?",
            answer: 2,
            options: [
              "Amazon EC2 costs are billed on a monthly basis.",
              "Users retain full administrative access to their Amazon EC2 instances.",
              "Amazon EC2 instances can be launched on demand when needed.",
              "Users can permanently run enough instances to handle peak workloads."
            ],
            description:`The ability to launch instances on demand when needed allows users to launch and terminate instances in
              response to a varying workload. This is a more economical practice than purchasing enough on-premises servers
              to handle the peak load.`,
            selected: null
        },
        {question: "Which AWS service would simplify the migration of a database to AWS?",
            answer: 1,
            options: [
              "AWS Storage Gateway",
              "AWS Database Migration Service (AWS DMS)",
              "Amazon EC2",
              "Amazon AppStream 2.0"
            ],
            description:`AWS DMS helps users migrate databases to AWS quickly and securely. The source database remains
              fully operational during the migration, minimizing downtime to applications that rely on the database. AWS DMS
              can migrate data to and from most widely used commercial and open-source databases.
              `,
            selected: null
        },
        {question:"Which AWS offering enables users to find, buy, and immediately start using software solutions in their AWS environment?",
            answer: 2,
            options: [
              "AWS Config",
              "AWS OpsWorks",
              "AWS SDK",
              "AWS Marketplace"
            ],
            description:`AWS Marketplace is a digital catalog with thousands of software listings from independent software
              vendors that makes it easy to find, test, buy, and deploy software that runs on AWS.`,
            selected: null
        },
        {question:"Which AWS networking service enables a company to create a virtual network within AWS?",
            answer: 2,
            options: [
              "AWS Config",
              "Amazon Route 53",
              "AWS Direct Connect",
              "Amazon Virtual Private Cloud (Amazon VPC)"
            ],
            description:`Amazon VPC lets users provision a logically isolated section of the AWS Cloud where users can launch
              AWS resources in a virtual network that they define.`,
            selected: null
        },
        {question: "Which AWS service would simplify the migration of a database to AWS?",
            answer: 1,
            options: [
              "AWS Storage Gateway",
              "AWS Database Migration Service (AWS DMS)",
              "Amazon EC2",
              "Amazon AppStream 2.0"
            ],
            description:`AWS DMS helps users migrate databases to AWS quickly and securely. The source database remains
              fully operational during the migration, minimizing downtime to applications that rely on the database. AWS DMS
              can migrate data to and from most widely used commercial and open-source databases.
              `,
            selected: null
        },
    ];

const questions=ref(questions_array.sort((a, b) => 0.5 - Math.random()))
  
//    const x=ref(4);
   
//    const y=computed(()=>{
//     return x;
//    });
//    const changeX =()=>{
//       x.value=8;
//     }

    const wrongAnswers=[];

    const quizCompleted = ref(false)

    const currentQuestion = ref (0)

    // console.log(questions.value[1].question);
    const score = computed (() => {
      let value = 0
      questions.value.map(q => {
        if (q.selected == q.answer) {
            value++
          }
      })
      return value
    })  

    const getCurrentQuestion = computed (() => {
      let question = questions.value [currentQuestion.value]
      question.index = currentQuestion.value
    //   question.x=5;
    //   console.log(question.x);
      return question
    })
    
    const SetAnswer = evt => {
      questions.value [currentQuestion.value].selected = evt.target.value
      evt.target.value = null

      if(getCurrentQuestion.value.selected!=getCurrentQuestion.value.answer){
          wrongAnswers.push([getCurrentQuestion.value.selected,currentQuestion.value]);
      }
      // console.log(getCurrentQuestion.value.answer);
      // console.log(questions.value[0].answer);
    }

    const NextQuestion = () => {
      if (currentQuestion.value < questions.value.length-1) {
          currentQuestion.value++
      } else {
          quizCompleted.value = true
      }
    }

 

</script>

<template>
  <body>
    <main class="app">
      <h1>The Quiz</h1>
      <div class="stepper">
        <div class="circle active">1</div>
        <div class="circle active">2</div>
        <div :class="`circle ${quizCompleted == true?'active':''}`">3</div>
      </div>
      <section class="quiz" v-if="!quizCompleted">
          <div class="quiz-info">
              <span class="question">{{ getCurrentQuestion.question }}</span>
              <span class="score">Score {{ score }}/{{ questions.length}}</span>
          </div>
          <div class="options">
              <label
                  v-for="(option, index) in getCurrentQuestion.options"
                  :key="index"
                  :class="`option ${
                        getCurrentQuestion.selected == index
                           ? index == getCurrentQuestion.answer
                              ? 'correct'
                              : 'wrong'
                        : ''
                  } ${
                        getCurrentQuestion.selected != null &&
                        index != getCurrentQuestion.selected
                        ? 'disabled'
                        :''
                    }`">
                  <input 
                      type="radio"
                      :name="getCurrentQuestion.index"
                      :value="index"
                      v-model="getCurrentQuestion.selected"
                      :disabled="getCurrentQuestion.selected"
                      @change="SetAnswer">
                  <span> {{ option }}</span>
              </label>
          </div>

          <button
              @click="NextQuestion"
              :disabled="! getCurrentQuestion.selected">
              {{
                  getCurrentQuestion.index == questions.length - 1
                      ? 'Finish'
                      : getCurrentQuestion.selected == null
                          ? 'Select an option'    
                          : 'Next Question'
              }}
          </button>

          <div class="progressBar">
            <span v-for="(question,index) in questions" :class="` ${currentQuestion>=index?'enabled':''}`">{{index+1}}</span>
          </div>
      </section>

      <section v-else>
        <h2>You have finished the quiz!</h2>
        <p>Your score is {{ score }} / {{ questions.length}}</p>
        <div class="result">
          <div v-for="(wrongAnswer,index) in wrongAnswers" key="index">
            <p><span class="wrong-answer">you chose:</span> {{questions[wrongAnswer[1]].options[wrongAnswer[0]]}}</p>
            <p><span class="right-answer">corret answer: </span>{{questions[wrongAnswer[1]].options[questions[wrongAnswer[1]].answer]}}</p>
            <p><span class="description">description:</span> {{questions[wrongAnswer[1]].description}}</p>
         </div>
        </div>
        
        </section>
  </main>
  </body>
    
</template>

<style scoped>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;
  font-family: 'Montserrat',sans-serif;

}

body{
  overflow-x: hidden;
  background-color:#271C36;
  color:#FFF;
  width: 100vw;
}

.app{
  display: flex;
  flex-direction: column;
  align-items:center;
  padding:2rem;
  min-height:100vh;
  width: 100vw;
}

h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}

.stepper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 280px;
  margin: 0px 0px 20px 0px;
}

.circle {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #ddd;
  color: #333;
  font-size: 20px;
}

.circle.active {  
  background-color: #2cce7d;
  color: #fff;
}

.quiz{
  background-color: #382a4b;
  padding:1rem;
  width:100%;
  max-width:640px;
}


.quiz-info{
  display:flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question{
  color: #8F8F8F;
  font-size:1.25rem;
}

.quiz.info .score{
  color:#FFF;
  font-size: 1.25rem;
}

.options{
  margin-bottom: 1rem;
}

.option{
  padding: 1rem;
  display: block;
  background-color:#271C36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover{
  background-color:#2d213f;
}

.option.correct{
  background-color: #2cce7d;
}

.option.wrong{
  background-color: #ff5a5f;
}

.option:last-of-type{
  margin-bottom:0;
}

input.disabled{
  opacity:0.5;
}

.option input{
  display:none;
}

button{
  appearance: none;
  outline:none;
  border:none;
  cursor:pointer;

  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size:1.25rem;
  border-radius: 0.5rem;
}

button:disabled{
  opacity: 0.5;
}

h2{
  font-size: 2rem;
  margin-bottom:2rem;
  text-align:center;
}

p{
  color: #8f8f8f;
  font-size: 1.25rem;
  text-align:center;
}

.progressBar {
  margin: 14PX 0PX 14PX 22PX;
}
.progressBar span {
  font-weight: bold;
  font-size: 20px;
  margin-right: 10px;
  color: #2cce7d;
  opacity: 0.5;
}

.enabled{
  opacity: 1!important;
}

.result{
  margin-top: 30px;
  width: 770PX
}
.result div{
  padding-top: 20px;
}
.result p{
text-align: start;
}
.result p .wrong-answer{
  color: #f84906;
}
.result p .right-answer{
  color: #2cce7d;
}
.result p .description{
  color:#2cce7d;
}
</style>



