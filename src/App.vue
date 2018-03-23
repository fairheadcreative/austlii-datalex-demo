<template>
  <div id="app">
    <section class="main-content">
      <div class="container">
        <div class="consultation">
          <div class="chat-view" ref="chatView">
            <div class="chat-message" v-for="message in messages" :key="message.id"
              :class="{ 'is-consultant': message.from === 'consultant' }">
              {{ message.text }}
            </div>
          </div>
          <div class="form-elements">
            <input type="text" v-model="newMessage" @keyup.enter="sendMessage">
            <input type="submit" value="Submit" @click="sendMessage">
          </div>
        </div>
        <div class="navigation">

          <div class="related">
          <div class="navigation-titlebar">
            <h4>Related Materials</h4>
          </div>
            <ul>
              <li v-for="material in materials" :key="material.id">{{ material.type }}: {{ material.title }}</li>
            </ul>
          </div>

          <div class="facts">
          <div class="navigation-titlebar">
            <h4>Facts</h4>
          </div>
            <ul>
              <li v-for="message in messages" :key="message.id" v-if="message.from === 'me'">You said: {{ message.text }}</li>
            </ul>
          </div>

          <div class="conclusions">
          <div class="navigation-titlebar">
            <h4>Conclusions</h4>
          </div>
            <ul>
              <li v-for="conclusion in conclusions" :key="conclusion.id">{{ conclusion.title }}</li>
            </ul>
          </div>

        </div>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  components: {},
  data () {
    return {
      newMessage: '',
      messages: [],
      materials: [],
      conclusions: [],
      lastMessageId: 0,
      lastMaterialId: 0,
      lastConclusionId: 0,
      endpoints: [
        'static/responses/response1.json',
        'static/responses/response2.json',
        'static/responses/response3.json',
        'static/responses/response4.json',
        'static/responses/response5.json',
        'static/responses/response6.json'
      ]
    }
  },
  methods: {
    getApiEndpoint () {
      return this.endpoints[Math.floor(Math.random() * this.endpoints.length)]
    },
    addMessage (from, text) {
      this.messages.push({
        id: ++this.lastMessageId,
        from: from,
        text: text
      })

      this.$nextTick(() => {
        this.$refs.chatView.scrollTop = this.$refs.chatView.scrollHeight - this.$refs.chatView.clientHeight
      })
    },
    addMaterial (material) {
      this.materials.push(
        Object.assign({ id: ++this.lastMaterialId }, material)
      )
    },
    addConclusion (conclusion) {
      this.conclusions.push(
        Object.assign({ id: ++this.lastConclusionId }, conclusion)
      )
    },
    parseAnswer (data) {
      this.addMessage('consultant', data.text)

      if (data.material) {
        this.addMaterial(data.material)
      }

      if (data.conclusion) {
        this.addConclusion(data.conclusion)
      }
    },
    sendMessage () {
      const newMessage = this.newMessage.trim()

      if (newMessage === '') {
        return
      }

      this.addMessage('me', newMessage)

      this.newMessage = ''

      axios.get(this.getApiEndpoint()).then(res => {
        this.parseAnswer(res.data)
      }).catch(err => console.log(err))
    }
  }
}
</script>

<style lang="scss">
body {
  padding: 0;
  margin: 0;
  font-family: sans-serif;
  background: #fafafa;
}

.container {
  max-width: 1024px;
  margin: 0 auto;
  overflow: auto;
  border: 1px solid #eaeaea;
  background: white;
}

.consultation {
  float: left;
  width: 65%;
  box-sizing: border-box;
  height: 80vh;
  position: relative;
}

.chat-view {
  float: left;
  width: 100%;
  padding: 1em;
  box-sizing: border-box;
  overflow: auto;
  height: calc(80vh - 54px);
}

.chat-message {
  padding: 1em;
  border-radius: 3px;
  margin-bottom: 1em;
  width: 70%;
  background: #F32928;
  color: white;
  float: right;

  &.is-consultant {
    background: #F5F5F5;
    float: left;
    color: black;
  }
}

.form-elements {
  position: absolute;
  bottom: 0;
  width: 100%;
  background: white;
  padding: 1em;
  box-sizing: border-box;
  border-top: 1px solid #eaeaea;
  text-align: right;
}

input {
  border: 1px solid silver;
  padding: 2px;
}

input[type="submit"] {
  background: white;
  padding: 2px 10px;
}

input[type="text"] {
  width: 70%;
}

.navigation {
  float: left;
  width: calc(35% - 1em);
  margin-left: 1em;
  background: #2E2E2E;
  color: #e9e9e9;
  box-sizing: border-box;
  height: 80vh;
  overflow: auto;
}

.navigation-titlebar {
  padding: .5em 1em;
  background: #383838;
  border: 1px solid rgba(255,255,255,.2);
  line-height: 1;
}

h4 {
  font-size: .85em;
  margin: 0;
  font-weight: 300;
  text-transform: uppercase;
}

.navigation ul li {
  padding: 1em 2.5em;
  font-size: .75em;
}

ul {
  margin: 0;
  padding: 0;
}

ul li {
  list-style: none;
}
</style>
