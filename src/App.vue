<template>
  <div id="app">
    <h1 class="page-header">Markdown Editor</h1>
    <div class="page-content">
      <MarkdownEditor />
    </div>
  </div>
</template>

<script>
import MarkdownEditor from "./components/MarkdownEditor";

export default {
  name: "App",
  components: {
    MarkdownEditor,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.page-header{
  text-align: center;
  border: solid blue;
  border-width: thin;
}

.page-content {
  flex-direction: column;
}
</style>
