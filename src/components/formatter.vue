<template>
  <img class="logo" src="/public/html-5.svg" alt="HTML5" />
  <div id="-inner-app">
    <h1>Terms Formatter for WordPress</h1>

    <!-- Input Section -->
    <div>
      <p>Paste Rich Text below</p>
      <div class="rich-text-input" contenteditable="true" @input="handleInput" ref="richTextInput"></div>
    </div>

    <!-- Formatted HTML Section -->
    <div>
      <p>Formatted HTML</p>
      <textarea v-model="formattedHTML" readonly class="formatted-output"></textarea>
    </div>

    <!-- Actions -->
    <button @click="copyToClipboard">Copy to Clipboard</button>
  </div>
</template>

<script>
import sanitizeHtml from "sanitize-html";

export default {
  data() {
    return {
      rawInput: "", // Stores raw HTML
      formattedHTML: "", // Stores formatted HTML
    };
  },
  methods: {
    handleInput() {
      // Get the raw HTML from the contenteditable div
      const rawHTML = this.$refs.richTextInput.innerHTML;

      // Sanitize and format the HTML
      const sanitizedHTML = sanitizeHtml(rawHTML, {
        allowedTags: ['b', 'i', 'em', 'strong', 'a', 'ol', 'li', 'h1', 'h2', 'p'],
        allowedAttributes: {
          a: ["href", "name", "target"],
          ol: ["type", "start"],
        },
      });

      // Set the formatted HTML
      this.formattedHTML = sanitizedHTML;
    },
    copyToClipboard() {
      navigator.clipboard.writeText(this.formattedHTML).then(() => {
        alert("HTML copied to clipboard!");
      });
    },
  },
};
</script>


<style>
body {
  background-color: cadetblue;
  color: whitesmoke;
}

#app {
  font-family: Arial, sans-serif;
  margin: 20px;
}

.logo {
  max-width: 3rem;
}

.rich-text-input {
  border: 1px solid #ccc;
  padding: 10px;
  min-height: 100px;
  background: #f9f9f9;
}

.formatted-output {
  width: 100%;
  min-height: 250px;
  margin-top: 10px;
  background: #f4f4f4;
  border: 1px solid #ddd;
}

button {
  margin-top: 10px;
  margin-right: 5px;
  padding: 10px 15px;
  cursor: pointer;
  background-color: cadetblue;
  color: #fff;
  border: 1px solid whitesmoke;

  &:hover {
    background-color: rgb(79, 159, 162)
  }
}
</style>
