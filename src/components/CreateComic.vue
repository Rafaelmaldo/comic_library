<template>
  <div
    class="bg-sky-50 p-5 rounded-lg shadow-md w-[500px] gap-2.5 flex flex-col items-center justify-center h-max mt-7 mx-auto"
  >
    <h1
      class="text-3xl font-bold mb-5 bg-gradient-to-br from-amber-500 to-amber-200 text-transparent bg-clip-text"
    >
     Add Comic +
    </h1>
    <BasicInput
      id="title"
      type="text"
      label="title"
      :showError="showErrors && !title"
      v-model="title"
    />
    <BasicInput
      id="number"
      type="number"
      label="number"
      :showError="showErrors && !number"
      v-model="number"
    />
    <BasicInput
      id="artist"
      type="text"
      label="artist"
      :showError="showErrors && !artist"
      v-model="artist"
    />
    <BasicInput
      id="writer"
      type="text"
      label="writer"
      :showError="showErrors && !writer"
      v-model="writer"
    />
    <BasicInput
      id="description"
      type="text"
      label="description"
      :showError="showErrors && !description"
      v-model="description"
    />
    <BasicInput
      id="image_url"
      type="text"
      label="image_url"
      :showError="showErrors && !image_url"
      v-model="image_url"
    />
    <button
      class="bg-sky-300 text-white py-2 px-4 rounded-lg hover:bg-sky-500 transition duration-300 ease-in-out focus:outline-none focus:ring-2 focus:ring-offset-2 press:ring-amber-500 focus:ring-opacity-50 group mt-2"
      type="submit"
      @click="createComicBook"
    >
      Create Comic
    </button>
  </div>
</template>
<script>
import BasicInput from "./BasicInput.vue";

export default {
  name: "CreateComic",
  components: {
    BasicInput,
  },
  data() {
    return {
      title: "",
      number: null,
      artist: "",
      writer: "",
      description: "",
      image_url: "",
      showErrors: false,
    };
  },
  methods: {
    async createComicBook() {
      if (
        !this.title ||
        !this.number ||
        !this.artist ||
        !this.writer ||
        !this.description ||
        !this.image_url
      ) {
        this.showErrors = true;
        return;
      } else {
        this.showErrors = false;
      }
      const comicBookData = {
        title: this.title,
        number: this.number,
        artist: this.artist,
        writer: this.writer,
        description: this.description,
        image_url: this.image_url,
      };

      await fetch("http://localhost:3000/comicbooks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ comicbook: comicBookData }),
      });

      this.$emit("refresh");
      this.clearInputs();
    },
    clearInputs() {
      this.title = "";
      this.number = "";
      this.number = null;
      this.artist = "";
      this.writer = "";
      this.description = "";
      this.image_url = "";
    },
  },
};
</script>
