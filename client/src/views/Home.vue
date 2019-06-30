<template>
  <div>
    <button @click="showMessageForm = !showMessageForm" type="button" class="btn btn-danger mt-3 mb-3">Write A Message</button>
    <form v-if="showMessageForm" @submit.prevent ="addMessage" class="mb-3">
      <div v-if="error" class="alert alert-dismissible alert-danger">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        <strong>Error: </strong>{{error}}
      </div>
      <div class="for
      m-group">
        <label for="username">Username</label>
        <input v-model="message.username" type="text" class="form-control" id="username" required>
      </div>
      <div class="form-group">
        <label for="subject">Subject</label>
        <input v-model="message.subject" type="text" class="form-control" id="subject" placeholder="Enter a subject" required>
      </div>
      <div class="form-group">
        <label for="message">Message</label>
        <textarea v-model="message.message" class="form-control" id="message" rows="3" required></textarea>
      </div>
      <div class="form-group">
        <label for="imageURL">Image URL</label>
        <input v-model="message.imageURL" type="URL" class="form-control" id="imageURL" placeholder="Image URL">
      </div>
      <button type="submit" class="btn btn-success">Add your message</button>
    </form>
    <hr>
    <ul class="list-unstyled">
      <div v-for="message in reversedMessages" :key="message._id">
      <li class="media">
        <img v-if="message.imageURL" class="mr-3" :src="message.imageURL" :alt="message.subject">
        <div class="media-body">
          <h4 class="mt-0 mb-1">{{message.username}}</h4>
          <h5 class="mt-0 mb-1">{{message.subject}}</h5>
          {{message.message}}
          <br />
          <small>{{message.created}}</small>
        </div>
      </li>
      <hr>
      </div>
    </ul>
  </div>
</template>

<script>
const API_URL = 'http://localhost:1234/messages';

export default {
  name: 'home',
  data: () => ({
    showMessageForm: false,
    error: '',
    messages: [],
    message: {
      username: 'Anonymous',
      subject: '',
      message: '',
      imageURL: '',
    },
  }),
  computed: {
    reversedMessages() {
      return this.messages.slice().reverse();
    },
  },
  mounted() {
    fetch(API_URL).then(response => response.json()).then((result) => {
      this.messages = result;
    });
  },
  methods: {
    addMessage() {
      fetch(API_URL, {
        method: 'POST',
        body: JSON.stringify(this.message),
        headers: {
          'content-type': 'application/json',
        },
      }).then(response => response.json()).then((result) => {
        if (result.details) {
          const error = result.details.map(detail => detail.message).join('. ');
          this.error = error;
        } else {
          this.error = '';
          this.showMessageForm = false;
          this.messages.push(result);
        }
      });
    },
  },
};
</script>

<style>
hr {
  border-top: 1px solid black;
}

img {
  max-width: 35%;
  height: auto;
}
</style>
