<template lang="pug">
.container
  textarea.post-message(v-model="tweet" placeholder="いまのキモチ。")
  div.postbutton
    v-ons-button(@click="post()" modifier="cta")
      | post
    .nk-list-group
      .nk-list-item(v-for="t in timeline")
        div.nk-timeline-area
          | {{t.tweet}}
          | {{t.date}}
        div.nk-les-post-area
          input.nk-les-text-area(v-model="les" type="text")
          button.nk-les-button(@click="lespost()")
            |res
          .nk-list-group
            .nk-list-item(v-for="t in lesline")
              | {{t.les}}
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
import 'onsenui/css/onsenui.css'
import 'onsenui/css/onsen-css-components.css'
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
      les: '',
      lesline: [],
      lesList: []
    }
  },
  methods: {
    post(){
      if (this.isTweetPresent()){
        this.tweetList.push({
          tweet: this.tweet,
          date: this.tweetTime()
        })
        //console.log(this.tweetList)
        this.timeline = this.reverse()
        this.clear()
      }
    },
    lespost(){
      if (this.isLesponsePresent()){
        //console.log("lespost呼ばれたやで")
        this.lesList.push({
          les: this.les
        })
        this.lesline = this.lesreverse()
        this.clear()
      }
    },
    clear(){
      this.tweet = ''
      this.les = ''
    },
    reverse(){
      var copy = this.tweetList.slice()
      return copy.reverse()
    },
    lesreverse(){
      var lescopy = this.lesList.slice()
      return lescopy.reverse()
    },
    isTweetPresent() {
      return this.tweet !== ''
    },
    tweetTime(){
      var date = moment()
      return date.format("HH:mm")
    },
    isLesponsePresent(){
      return this.les !==''
    }
  }
}
</script>
