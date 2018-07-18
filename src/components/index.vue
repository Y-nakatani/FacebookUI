<template lang="pug">
.container
  textarea.post-message(v-model="tweet" placeholder="いまのキモチ。")
  div.postbutton
    v-ons-button(@click="post()" modifier="cta")
      | post
    .nk-list-group
      .nk-list-item(v-for="(t, i) in timeline")
        div.nk-timeline-area
          | {{t.tweet}}
          | {{t.date}}
          | {{i}}
        div.nk-les-post-area
          input.nk-les-text-area(v-model="message[i]" type="text")
          button.nk-les-button(@click="lespost(i)")
            |res
          .nk-list-group
            .nk-list-item(v-for="l in t.lesList")
              | {{l.les}}
</template>

<style lang="sass">
  *:focus
    outline: none
  .container
    margin: 0 auto
    width: 30em
  .post-message
    margin: 10px 0 0 0
    width: 40em
    height: 6em
    font-size: 10px
  .nk-list-item
    margin: 20px 0 0 0
    padding: 10px
    border: solid
    text-align: left
    border-color: skyblue
    border-radius: 10px
  .postbutton
    text-align: center
  .nk-timeline-area
    padding: 0 0 20px 0
    border-bottom: solid 0.5px
    border-color: inherit
    margin: 0 0 10px 0
  .nk-les-post-area
    text-align: right
  .nk-les-button
    margin: 0 0 0 5px

</style>

<script>
import moment from 'moment'
// Webpack CSS import
// JS import
import Vue from 'vue'
import VueOnsen from 'vue-onsenui'
Vue.use(VueOnsen);


export default{
  data(){
    return{
      tweet: '',
      tweetList: [],
      timeline: [],
      message: []
    }
  },
  methods: {
    post(){
      if (this.isTweetPresent()){
        this.tweetList.push({
          tweet: this.tweet,
          date: this.tweetTime(),
          lesList: []
        })
        this.message.push('')
        this.timeline = this.reverse()
        this.clear()
      }
    },
    lespost(index){
      if (this.isLesponsePresent()){
        //console.log("lespost呼ばれたやで")
        this.timeline[index].lesList.push({
          les: this.message[index]
        })
        this.message[index] = ''
        this.clear()
      }
    },
    clear(){
      this.tweet = ''
    },
    reverse(){
      var copy = this.tweetList.slice()
      return copy.reverse()
    },
    isTweetPresent() {
      return this.tweet !== ''
    },
    tweetTime(){
      var date = moment()
      return date.format("HH:mm")
    },
    isLesponsePresent(){
      return this.message !==''
    }
  }
}
</script>
