<template>
  <div class="container mx-auto p-10">
    <div
      class="font-bold text-center text-3xl border p-6 rounded shadow-md bg-gradient-to-r from-cyan-500 to-blue-500 text-white rounded-xl"
    >
      PDF Processor App
    </div>

    <div
      class="font-light text-center text-xl p-6 grid grid-rows-1 grid-flow-col gap-4"
    >
      <div
        class="border-4 border-sky-300 rounded-xl p-6 font-normal shadow-md bg-gradient-to-r from-cyan-500 to-blue-500 text-white"
      >
        Image to PDF
      </div>
      <div
        class="border rounded-xl p-6 font-normal shadow-md bg-gradient-to-r from-cyan-500 to-blue-500 text-white"
      >
        Lock PDF (Coming Soon)
      </div>
      <div
        class="border rounded-xl p-6 font-normal shadow-md bg-gradient-to-r from-cyan-500 to-blue-500 text-white"
      >
        HTML to PDF (Coming Soon)
      </div>
    </div>
    <hr />
    <section class="my-10">
      <div
        class="mt-2 flex justify-center rounded-lg border border-dashed border-gray-900/25 px-6 py-10"
      >
        <div class="text-center">
          <PhotoIcon
            class="mx-auto h-12 w-12 text-gray-300"
            aria-hidden="true"
          />
          <div class="mt-4 flex text-sm leading-6 text-gray-600">
            <label
              for="file"
              class="relative cursor-pointer rounded-md bg-white font-semibold text-indigo-600 focus-within:outline-none focus-within:ring-2 focus-within:ring-indigo-600 focus-within:ring-offset-2 hover:text-indigo-500"
            >
              <span>Upload a file</span>
              <input
                id="file"
                name="file"
                type="file"
                class="sr-only"
                @change="onChange"
              />
            </label>
            <p class="pl-1">or drag and drop</p>
          </div>
          <p class="text-xs leading-5 text-gray-600">PNG, JPG up to 10MB</p>
        </div>
      </div>
    </section>
    <div
      v-if="result.link"
      class="mt-2 flex justify-center px-6 py-1 text-white font-bold"
    >
      <button
        @click="downloadFile()"
        class="border p-2 rounded-lg bg-gradient-to-r from-cyan-500 to-blue-500"
      >
        Download
      </button>
    </div>
  </div>
</template>

<script setup>
const result = reactive({
  link: "",
});

function downloadFile() {
  window.open(result.link);
}

const onChange = async (e) => {
  const files = e.target.files;
  const formData = new FormData();
  formData.append("file", files[0]);
  await useFetch("http://localhost:3002/api/upload", {
    method: "post",
    body: formData,
  }).then((response) => {
    const blob = new Blob([response.data.value], { type: "application/pdf" });
    const objectUrl = window.URL.createObjectURL(blob);
    result.link = objectUrl;
    elt.impressionEnCours = false;
  });
};
</script>

<style></style>
