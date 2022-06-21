<template>
  <div class="bg-white my-5 p-5 rounded-lg">
    <div class="card">
      <div class="flex flex-row justify-start gap-2 items-center">
        <DoctorIcon class="w-16 rounded-full bg-violet-200" />
        <div class="flex flex-col">
          <strong>{{ profile.name }}</strong>
          <a :href="profile.email" class="email">{{ profile.email }}</a>
        </div>
      </div>
      <div class="text-violet-500 my-2 flex flex-col">
        <p v-for="(item,index) in description" :key="index">
          {{item}}
        </p>
      </div>

      <div class="flex flex-row justify-start items-center">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 text-sky-500"
          viewBox="0 0 20 20"
          fill="currentColor"
        >
          <path
            fill-rule="evenodd"
            d="M3.172 5.172a4 4 0 015.656 0L10 6.343l1.172-1.171a4 4 0 115.656 5.656L10 17.657l-6.828-6.829a4 4 0 010-5.656z"
            clip-rule="evenodd"
          />
        </svg>
        <p class="ml-2">{{ profile.likes }}</p>
      </div>
    </div>
    <div class="flex flex-row mt-2">
      <button
        class="p-2 basis-1/2 flex items-center justify-center"
        @click="like()"
      >
        <svg
          :class="{ 'fill-green-500': likeCounter % 2 == 1 }"
          class="h-8 w-8 text-green-500"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M14 10h4.764a2 2 0 011.789 2.894l-3.5 7A2 2 0 0115.263 21h-4.017c-.163 0-.326-.02-.485-.06L7 20m7-10V5a2 2 0 00-2-2h-.095c-.5 0-.905.405-.905.905 0 .714-.211 1.412-.608 2.006L7 11v9m7-10h-2M7 20H5a2 2 0 01-2-2v-6a2 2 0 012-2h2.5"
          />
        </svg>
      </button>
      <button
        class="p-2 basis-1/2 flex items-center justify-center"
        @click="dislike()"
      >
        <svg
          :class="{ 'fill-red-500': disLikeCounter % 2 == 1 }"
          class="h-8 w-8 text-red-500"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M10 14H5.236a2 2 0 01-1.789-2.894l3.5-7A2 2 0 018.736 3h4.018a2 2 0 01.485.06l3.76.94m-7 10v5a2 2 0 002 2h.096c.5 0 .905-.405.905-.904 0-.715.211-1.413.608-2.008L17 13V4m-7 10h2m5-10h2a2 2 0 012 2v6a2 2 0 01-2 2h-2.5"
          />
        </svg>
      </button>
    </div>
    <div class="">
      <input class="outline-none" placeholder="Write your comment..." />
    </div>
  </div>
</template>

<script>
import DoctorIcon from "./DoctorIcon";

export default {
  name: "ProfileCard",
  data() {
    return {
      likeCounter: 0,
      disLikeCounter: 0,
      maindata: this.profile.likes,
    };
  },
  computed:{
    description(){
      return this.profile.description.split(',')
    }
  },
  methods: {
    like() {
      const likeMode = this.likeCounter % 2;
      const disLikeMode = this.disLikeCounter % 2;
      if (likeMode == 0 && disLikeMode == 1) {
        this.disLikeCounter++;
        this.$emit("like", this.maindata + 1);
      } else if (likeMode == 0) {
        this.$emit("like", this.maindata + 1);
      } else if (likeMode == 1) {
        this.$emit("like", this.maindata);
      }
      this.likeCounter++;
    },
    dislike() {
      const likeMode = this.likeCounter % 2;
      const disLikeMode = this.disLikeCounter % 2;
      if (disLikeMode == 0 && likeMode == 1) {
        this.likeCounter++;
        this.$emit("like", this.maindata - 1);
      } else if (disLikeMode == 0) {
        this.$emit("like", this.maindata - 1);
      } else if (disLikeMode == 1) {
        this.$emit("like", this.maindata);
      }
      this.disLikeCounter++;
    },
  },
  components: {
    DoctorIcon,
  },

  props: {
    profile: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style>
</style>
