<template>
  <div class="admin">
    <!-- onglet admin -->
    <div class="onglet-admin">
      <div class="logo-left">
        <img alt="Vue logo" src="../assets/logo-hako.png" class="logo-chat"> <h1><span class="white">hako</span><span class="orange">bio</span></h1>
      </div>
      <div class="chat-name">hako<span class="orange">bot</span> admin</div>
    </div>

    <div class="form-contenu">
      <div class="info">
        <div class="title">
          questions/answers/status
        </div>
        <div class="q-a">
          <p>Questions</p>
          <p>Answers</p>
          <p>Status</p>
        </div>
      </div>
      <div class="liste-questions" v-for="(q,index) in questions" :key="index">
        <form action="post" @submit.prevent="updateQuestion(q.id,q.question,q.answer,q.status)">
          <div class="champ">
            <textarea name="message" v-model="q.question" placeholder="question"></textarea>
            <textarea name="message" v-model="q.answer" placeholder="answer"></textarea>
            <textarea class="status" name="message" v-model="q.status" placeholder="status"></textarea>
          </div>
          <div class="button">
          <button class="submit" type="submit">Update</button>
          <button class="delete" type="button" @click="deleteQuestion(q.id)">Delete</button>
          </div>
        </form>
      </div>
      <div class="info">
        <div class="title">
          New questions/answers/status
        </div>
        <div class="q-a">
          <p>Questions</p>
          <p>Answers</p>
          <p>Status</p>
        </div>
      </div>
      <div class="new-q">
      <form action="post" @submit.prevent="newQuestion()" class="new-q">
        <div class="champ">
        <textarea name="message" v-model="questionToAdd.question" placeholder="question"></textarea>
        <textarea name="message" v-model="questionToAdd.answer" placeholder="answer"></textarea>
        <textarea class="status" name="message" v-model="questionToAdd.status" placeholder="status"></textarea>
        </div>
        <button class="submit" type="submit">New</button>
      </form>
      </div>
    </div>

  </div>
</template>

<script>
import axios from 'axios'


export default {
  data(){
    return{
      questions: [],
      questionToAdd: [{
        answer: "",
        question: "",
        status:""
      }]
    }
  },
  created(){
    this.ask()
  },
  props: {
    msg: String
  },
  methods:{
    ask(){
      axios.get("https://ouatapi20190320032743.azurewebsites.net/api/questions")
      .then(response => {
        //this.answersTmp = response.data.answer.split('_@_')
        this.questions = response.data
        /*this.answersTmp.forEach((el) => {
        })*/
      })
    },
    deleteQuestion(id){
      let suppr = confirm("Voulez-vous vraiment supprimer cette question ?");

      if (suppr) {
        axios.delete("https://ouatapi20190320032743.azurewebsites.net/api/questions?id=" + id)
        .then(()=>{
          this.ask()
        });
      }
    },
    updateQuestion(id, question, answer, status){
      axios.put("https://ouatapi20190320032743.azurewebsites.net/api/questions", {"id":id,"answer":answer,"question":question, "status":status}).then();
    },
    newQuestion(){
      axios.post("https://ouatapi20190320032743.azurewebsites.net/api/questions", {"answer":this.questionToAdd.answer,"question":this.questionToAdd.question,"status":this.questionToAdd.status}).then(this.ask);
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.title{
  text-align: center;
  font-size: 1.5em;
  margin-bottom: 30px;
}
.info{
  width: 1200px;
}
.info .q-a{
  display: flex;
  justify-content: flex-start;
}
.q-a p{
  margin-right: 215px;
  font-weight: bold;
  color: #00B3B3;
}
form{
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #ececf5;
  margin-bottom: 20px;
  height: 90px;
  border-radius: 3px;
  width: 1200px;
  padding: 5px 20px 5px 15px;
}
.form-contenu{
  width: 80%;
  height: 60%;
  display: flex;
  flex-direction: column;
  /* justify-content: center; */
  align-items: center;
}
textarea{
  resize: none;
}
textarea, input{
  border: 1px solid #00B3B3;
  font-size: 1em;
  background: white;
  border-radius: 5px;
  padding: 10px;
  width: 250px;
  height: 70px;
  outline: none;
  font-family: Arial, Helvetica, sans-serif;
  margin: 5px;
  box-sizing: border-box;
}
.status{
  width: 200px;
}
button{
  width: 100px;
  height: 40px;
  padding: 10px;
  border: none;
  color: white;
  font-size: 1em;
  border-radius: 5px;
  margin-left: 10px;
}
.submit{
  background: #00B3B3;
}
.delete{
  background: #FF8047;
}
html, body{
  max-width: 100vw;
  box-sizing: border-box;
}
.admin{
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: Arial, Helvetica, sans-serif;
  margin-bottom: 100px;
}
.onglet-admin{
  position: absolute;
  width: 100%;
  max-width: 100vw;
  top: 0;
  height: 130px;
  background: #00B3B3;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 80px;
  padding-right: 80px;
  box-sizing: border-box;
}
.logo-left{
  display: flex;
  justify-content: center;
  align-items: center;
}
.logo-left img{
  width: 60px;
}
.logo-left h1{
  font-weight: 400;
  color: white;
  letter-spacing: 2px;
  text-transform: uppercase;
  font-size: 1.3em;
  margin-left: 10px;
}
.chat-name{
  font-size: 1.6em;
  text-transform: uppercase;
  letter-spacing: 2px;
  color: white;
}
.orange{
  color: #FF8047;
}


</style>
