<template>
  <div id="chat">
    <!-- bot image -->
    <div class="bot-img" @click="activeChat" v-bind:class="{close: !chatActive}">
      <img src="../assets/bullet.png" alt="" class="bullet-img">
      <img src="../assets/chat.png" alt="" class="bot">
    </div>
    <!-- boite chat -->
    <div class="box-chat" v-bind:class="{active: chatActive}">
      <!-- onglet top -->
      <div class="onglet-chat">
        <div class="logo-onglet">
          <img alt="Vue logo" src="../assets/logo-hako.png" class="logo-chat">
          <span class="chat-name">hako<span class="orange2">bot</span></span>
        </div>
        <i class="fas fa-times" @click="closeChat"></i>
      </div>
      <!-- champ bulles & questions -->
      <div class="chat-champ">
        <!-- champ bulles -->
        <div class="bullets-champ" id="truc">
          <div class="bullet bot-bullet">
            <img src="https://media.giphy.com/media/3oKIPsx2VAYAgEHC12/giphy.gif" alt="">
            <p>Hi! How can I help you?</p>
          </div>

          <div v-for="(a,index) in answers" :key="index" class="bullets-answers">

            <div class="bullet" :class="{'bot-bullet': (a.type == 'A'), 'admin-bullet': (a.type == 'Q')}" v-if="a.type == 'A' || a.type == 'Q'"><p>{{a.text}}</p></div>

            <div class="bullet bot-bullet" v-if="a.type == 'L'"><p><a :href="a.text" target="_blank">{{a.text}}</a></p></div>

            <div class="bullet bot-bullet" v-if="a.type == 'I'">
              <lightbox :thumbnail="a.text" :images="[{ link: a.text, alt: 'Cat 1' }
              ]"
              >
              <lightbox-default-loader slot="loader"></lightbox-default-loader>
              <div slot="content" slot-scope="{ url: { link, alt } }">
                <img :src="link" :alt="alt">
              </div>
            </lightbox>
            </div>

          </div>


          <div class="bullet bot-bullet" v-if="tmp"><p>...</p></div>


        </div> 
        <!-- champ question -->
        <form action="POST">
          <textarea name="message" id="message" cols="30" rows="5" placeholder="Ask me anything" v-model="questionTmp">
          </textarea>
          <div class="submit" @click="ask"><i class="fas fa-paper-plane"></i></div>
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
      // goodAnswer: '',
      chatActive: true,
      tmp: false,
      //question: "What is hakobio ?",
      //question: "Where can I find help ?",
      questionTmp: "How to create a room in HakoBio ?",
      question: "",
      answersTmp: [],
      answers: [],
      headers: {
        'Content-Type': 'application/json',
        'Authorization': 'EndpointKey 9525cf67-ed6d-4124-8b25-56e3248fa1f7' 
      },
      fakeAnswer: [
        {
          text: 'My question 1',
          type: 'Q'
        },
        {
          text: 'My answer 1',
          type: 'A'
        },
        {
          text: 'My question 2',
          type: 'Q'
        },
        {
          text: 'My answer 2.1',
          type: 'A'
        },
        {
          text: 'My answer 2.2',
          type: 'A'
        },
        {
          text: 'My answer 2.3',
          type: 'A'
        }
      ]
    }
  },
  created(){

  },
  props: {
    msg: String
  },
  methods:{
    // post(){
    //   this.$nextTick(() => {
    //       this.scrollToEnd();
    //     })
    // },
    activeChat(){
      this.chatActive = true;
    },
    closeChat(){
      this.chatActive = false;
    },
    ask(){
      this.$nextTick(() => {
          this.scrollToEnd();
        })


      // this.question = this.questions
      this.question = this.questionTmp
      this.questionTmp = ''
      this.answers.push({text:this.question, type: 'Q'})
      this.tmp = true
      this.$nextTick(() => {
        this.scrollToEnd();
      })
      //axios.post("https://qnamakerouat.azurewebsites.net/qnamaker/knowledgebases/cd2be153-3489-4ba1-a9c3-f20ae73a60a4/generateAnswer",{"question": this.question},{headers: this.headers})
      axios.get("https://ouatapi20190320032743.azurewebsites.net/api/chatbot/?question=" + this.question)
      .then(response => {
        this.tmp = false
        this.question = ''
        //this.answersTmp = response.data.answers[0].answer.split('_@_')
        this.answersTmp = response.data.answer.split('_@_')
        this.answersTmp.forEach((el) => {

          if(/(http(s?):)([/|.|\w|%|-])*/i.test(el)){
            if(/(http(s?):)([/|.|\w|\s|%|-])*\.(?:jpg|gif|png|jpeg|svg)/i.test(el)){
              this.answers.push({text:el, type: 'I'})
            }else{
              this.answers.push({text:el, type: 'L'})
            }
          }else{
            this.answers.push({text:el, type: 'A'})
          }




        })
        this.$nextTick(() => {
          this.scrollToEnd();
        })
      })
    },
    scrollToEnd: function() {
      var container = this.$el.querySelector('#truc');
      container.scrollTop = container.scrollHeight;
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
#chat{
  position: fixed;
  right: 0;
  bottom: 80px;
  font-family: Arial, Helvetica, sans-serif;
  display: flex;
  align-items: center;
}
.bot-img{
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  flex-direction: row;
  margin-bottom: 10px;
  transition: .3s all ease-in-out;
}
.bot{
  width: 80px;
  transition: .3s all ease-in-out;
}
.bullet-img{
  opacity: 0;
  width: 155px;
  right: -100px;
  transition: .3s all ease-in-out;
}
.bullet img{
  max-width: 100%;
}
.bot-img.close:hover{
  cursor: pointer;
}
.bot-img.close:hover .bot{
  width:110px;
}
.bot-img.close:hover .bullet-img{
  opacity: 1;
  right: 0;
}
.box-chat{
  width: 0;
  height: 570px;
  flex-direction: column;
  display: flex;
  opacity: 0;
  transition: .3s all ease-in-out;
  margin-right: 15px;
  margin-left: 10px;
}
.box-chat.active{
  right: 10px;
  opacity: 1;
  width: 330px;
}
.onglet-chat{
  height: 13%;
  width: 100%;
  border-radius: 10px 10px 0 0;
  background: #00B3B3;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-right: 20px;
  padding-left: 20px;
  box-sizing: border-box;
}
.orange2{
  color: rgb(255, 150, 102);
}
.logo-chat{
  width: 29px;
}
.chat-name{
  color: white;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-size: 1em;
  padding-left: 10px;
}
.logo-onglet{
  display: flex;
  align-items: center;
}
.chat-champ{
  height: 87%;
  background: rgb(190, 226, 226);
  border-radius: 0 0 10px 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.bullets-champ{
  padding: 15px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  max-height: 100%;
}
.bullet{
  max-width: 210px;
  padding: 10px;
  background: white;
  border-radius: 10px;
  color: black;
  margin-bottom: 15px;
  text-align: left;
}
.bullet p{
  margin: 0;
  overflow-wrap: break-word;
}
.admin-bullet{
  /* align-self: flex-end; */
  float: right;
}
form{
  display: flex;
  justify-content: space-between;
}
textarea{
  height: 80px;
  font-size: 1em;
  background: rgb(232, 247, 247);
  font-family: Arial, Helvetica, sans-serif;
  padding: 12px 15px;
  border: none;
  border-left: 1px solid rgb(190, 226, 226);
  border-bottom: 1px solid rgb(190, 226, 226);
  border-top: 1px solid rgb(190, 226, 226);
  box-sizing: border-box;
  overflow-y: auto;
  outline: none;
  resize: none;
  border-radius: 0 0 0 10px;
}
.submit{
  background-color: #00B3B3;
  color: white;
  font-size: 1.2em;
  border-radius: 0 0 10px 0;
  border: none;
  cursor: pointer;
  height: 80px;
  width: 85px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.fa-times{
  color: white;
  font-size: 1.2em;
  cursor: pointer;
}
.lightbox{
  height: 100vh !important;
}
.lightbox__image img{
  width: auto !important;
}
.lightbox__image div{
  display: flex !important;
  justify-content: center;
  align-items: center;
}
</style>
