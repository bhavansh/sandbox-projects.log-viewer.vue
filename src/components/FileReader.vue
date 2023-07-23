<template>
    <h1>File Reader</h1>
    <input type="file" @change="onFileInputChange" />
    <div>
       <pre v-if="fileContent" class="language-log okaidia">
        <code v-html="code"></code>
      </pre>
    </div>
  </template>

<script>
import { defineComponent } from 'vue';
import Prism from "prismjs";
import 'prismjs/themes/prism-okaidia.css'
import "prismjs/components/prism-log"

export default defineComponent({
  data() {
    return {
      fileContent: null,
      code: null,
    };
  },
  methods: {
    onFileInputChange(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();

        reader.onload = () => {
          // 'result' contains the file content as a data URL
          this.fileContent = reader.result;
          this.highlightCode(); // Call the function to apply syntax highlighting
        };

        // Read the file as text
        reader.readAsText(file);
      }
    },
    highlightCode() {

        // Preprocess the content to remove leading whitespaces
      const trimmedContent = this.fileContent
        .split('\n')
        .map((line) => line.trim())
        .join('\n');

      // Highlight the text in one go
      const highlightedText = Prism.highlight(trimmedContent, Prism.languages.log, 'log');
      console.log(highlightedText)
      // Insert the highlighted text into the code block
      this.code = highlightedText;

      
    },
  },
});
</script>

<style>
.container {
  max-width: 100%;
  margin: 3%;
  padding: 3%;
  box-sizing: border-box;
}

pre {
  height: 80vh; /* Set height to 75% of the viewport height */
  overflow: auto;
}
</style>