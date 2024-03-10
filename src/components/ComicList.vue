<template>
  <div
    class="py-5 flex flex-col items-center justify-center min-h-screen gap-5 w-max px-5"
  >
    <div
      v-for="comicbook in comicbooks"
      :key="comicbook.id"
      class="bg-sky-50 py-5 px-7 shadow-md rounded-lg overflow-hidden my-1 w-96 mx-auto relative"
    >
      <img
        v-if="editingId === comicbook.id"
        src="../assets/cancel.svg"
        alt="logo"
        class="w-5 absolute top-0 left-0 bg-white rounded-md shadow-md hover:transform hover:scale-125 transition duration-75 ease-in-out cursor-pointer ml-2.5 mt-2.5 p-px"
        @click.stop="editingId = null"
      />
      <img
        v-else
        src="../assets/trash-svgrepo-com.svg"
        alt="logo"
        class="w-5 absolute top-0 left-0 bg-white rounded-md shadow-md hover:transform hover:scale-125 transition duration-75 ease-in-out cursor-pointer ml-2.5 mt-2.5 p-px"
        @click="removeComicBook(comicbook.id)"
      />
      <img
        v-if="editingId === comicbook.id"
        src="../assets/noun-save-6667153.svg"
        alt="logo"
        class="w-5 absolute top-0 right-0 bg-green-400 rounded-md shadow-md hover:transform hover:scale-125 transition duration-75 ease-in-out cursor-pointer mr-2.5 mt-2.5 p-px"
        @click="updateComicBook(comicbook.id)"
      />
      <img
        v-else
        src="../assets/pencil.svg"
        alt="logo"
        class="w-5 absolute top-0 right-0 bg-white rounded-md shadow-md hover:transform hover:scale-125 transition duration-75 ease-in-out cursor-pointer mr-2.5 mt-2.5 p-px"
        @click="setCurrentlyEditing(comicbook.id)"
      />
      <img
        :src="comicbook.image_url"
        alt="comicbook cover"
        class="w-64 mx-auto bg-slate-300 object-cover border-spacing-2 border-4 border-slate-100 rounded-lg shadow-md"
      />
      <div v-if="editingId === comicbook.id">
        <BasicInput
          id="title"
          type="text"
          label="title"
          :showError="showErrors && !updatedComicBook.title"
          v-model="updatedComicBook.title"
        />
        <BasicInput
          id="number"
          type="number"
          label="number"
          :showError="showErrors && !updatedComicBook.number"
          v-model="updatedComicBook.number"
        />
        <BasicInput
          id="artist"
          type="text"
          label="artist"
          :showError="showErrors && !updatedComicBook.artist"
          v-model="updatedComicBook.artist"
        />
        <BasicInput
          id="writer"
          type="text"
          label="writer"
          :showError="showErrors && !updatedComicBook.writer"
          v-model="updatedComicBook.writer"
        />
        <BasicInput
          id="description"
          type="text"
          label="description"
          :showError="showErrors && !updatedComicBook.description"
          v-model="updatedComicBook.description"
        />
        <BasicInput
          id="image_url"
          type="text"
          label="image_url"
          :showError="showErrors && !updatedComicBook.image_url"
          v-model="updatedComicBook.image_url"
        />
      </div>
      <div v-else class="p-4 border-t border-gray-400 mt-2 pt-2">
        <h3 class="font-bold text-lg mb-2">
          {{ comicbook.title }} #{{ comicbook.number }}
        </h3>
        <p class="text-gray-700 text-sm">Artist: {{ comicbook.artist }}</p>
        <p class="text-gray-700 text-sm">Writer: {{ comicbook.writer }}</p>
        <p class="text-gray-600 text-sm mt-2">
          {{ comicbook.description }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import BasicInput from "./BasicInput.vue";
export default {
  name: "HelloWorld",
  components: {
    BasicInput,
  },
  data() {
    return {
      editingId: null,
      updatedComicBook: {
        title: "",
        number: null,
        artist: "",
        writer: "",
        description: "",
        image_url: "",
      },
      showErrors: false,
    };
  },
  props: {
    msg: String,
    comicbooks: Array,
  },
  methods: {
    async removeComicBook(id) {
      console.log("remove comicbook id =>", id);
      const response = await fetch(`http://localhost:3000/comicbooks/${id}`, {
        method: "DELETE",
      });
      console.log("response =>", response);
      this.$emit("refresh");
    },
    async updateComicBook() {
      if (
        !this.updatedComicBook.title ||
        !this.updatedComicBook.number ||
        !this.updatedComicBook.artist ||
        !this.updatedComicBook.writer ||
        !this.updatedComicBook.description ||
        !this.updatedComicBook.image_url
      ) {
        this.showErrors = true;
        return;
      } else {
        this.showErrors = false;
      }
      const response = await fetch(`http://localhost:3000/comicbooks/${this.editingId}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(this.updatedComicBook),
      });
      console.log("response =>", response);
      this.editingId = null;
      this.$emit("refresh");
    },
    setCurrentlyEditing(id) {
      this.updatedComicBook = {
        ...this.comicbooks.find((comicbook) => comicbook.id === id),
      };
      this.editingId = id;
    },
  },
};
</script>
