<template>
  <div class="chat-bot">
    <div v-for="(message, index) in messages" :key="index">
      <div v-if="message.from === 'bot'">
        <div class="chatbot-message chatbot-bot-message">{{ message.text }}</div>
        <div v-if="message.downloadLink" id="download-link">
            <a :href="message.downloadLink" download>{{ message.downloadLinkText }}</a>
        </div>
      </div>
      <div v-else>
        <div class="chatbot-message chatbot-user-message">{{ message.text }}</div>
      </div>
    </div>
    <form @submit.prevent="sendMessage">
      <input type="text" v-model="inputMessage" class="chatbot-input" />
      <button type="submit" class="chatbot-send-button">Send</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";
import './App.css';

axios.defaults.baseURL = 'http://localhost:8000';


export default {
  name: "Chat-bot",
  data() {
    return {
      inputMessage: "",
      messages: [],
    };
  },
  methods: {
    async sendMessage() {
      if (this.inputMessage.trim() === "") {
        return;
      }
      this.messages.push({
        from: "user",
        text: this.inputMessage.trim(),
      });
      this.inputMessage = "";
      try {
        const urlRegex = /^(https?:\/\/)?([\da-z.-]+)\.([a-z.]{2,6})(\/[^\s]*)?$/;
        const isUrl = urlRegex.test(this.messages[this.messages.length - 1].text);
        if (isUrl) {
            try {
                const response = await axios.post("/articles", {
                    url: this.messages[this.messages.length - 1].text,
                });
                const article = response.data;

                // Create a blob with the title and content
                const blob = new Blob([`Title: ${article.title}\n\n${article.content}`], {type: "text/plain;charset=utf-8"});

                // Create a download link
                const downloadLink = document.createElement("a");
                downloadLink.href = URL.createObjectURL(blob);
                downloadLink.download = `${article.title}.txt`;
                downloadLink.textContent = `Download ${article.title}.txt`;

                this.messages.push({
                    from: "bot",
                    text: `Article title: "${article.title}"`,
                    downloadLink: downloadLink,
                    downloadLinkText: `Download "${article.title}" as text file`,
                    });
            } catch (error) {
                console.error(error);
                    this.messages.push({
                        from: "bot",
                        text: "Sorry, Could not process the url.",
                });
            }
        } else {
            try {
                console.log(this.messages[this.messages.length - 1].text);
                const response = await axios.get("/articles", {
                    params: {
                        query: this.messages[this.messages.length - 1].text,
                    }
                });
                const articles = response.data;
                console.log(articles[0].title);

                for (var i = 0; i < articles.length; i++) {
                    console.log(articles[i].url);

                    var article = articles [i]
                    this.messages.push({
                        from: "bot",
                        text: `The text found in URL: "${article.url}"`,
                    });
                    this.messages.push({
                        from: "bot",
                        text: `Text Snippets: "${article.snippets}"`,
                    });
                }
            } catch (error) {
                console.error(error);
                    this.messages.push({
                    from: "bot",
                    text: "Sorry, I couldn't find any articles that have the given text.",
                });
            }
        }
      } catch (error) {
        console.error(error);
        this.messages.push({
          from: "bot",
          text: "Unknown error occurred!",
        });
      }
    },
  },
};
</script>

