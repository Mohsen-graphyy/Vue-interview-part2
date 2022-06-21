<template>
  <div
    class="
      bg-gradient-to-r
      from-violet-400
      to-blue-400
      flex
      justify-center
      p-5
      md:p-20
      items-center
    "
  >
    <div
      class="
        container
        mx-auto
        px-5
        md:px-48
        py-10
        rounded-lg
        bg-white bg-opacity-20
      "
    >
      <div class="text-center font-bold text-3xl text-white">Profiles List</div>
      <div class="">
        <div class="flex flex-row gap-5 justify-between items-center my-10">
          <button class="basis-1/2 p-3 bg-white rounded-lg" @click="sortAsc">
            ▲
          </button>
          <button class="basis-1/2 p-3 bg-white rounded-lg" @click="sortDesc">
            ▼
          </button>
        </div>
        <div class="flex flex-col gap-2">
          <label class="text-lg font-bold" for="filter">Find profile:</label>
          <input
            class="p-3 outline-none bg-gray-100 rounded-lg"
            placeholder="Search Profile"
            v-model="searchText"
          />
        </div>
        <ProfileCard
          v-for="profile in search"
          @like="clickCall($event,profile.id)"
          :key="profile.id"
          :profile="profile"
          class="profile"
        />
        <p
          class="text-2xl text-center my-10 font-semibold"
          v-show="search.length == 0"
        >
          Not Found {{ searchText }}
        </p>
      </div>
      <hr class="my-10">
      <div class="mt-10">
        <p class="text-white font-bold text-3xl text-center">Add new profile</p>
        <div class="flex flex-col gap-5">
          <div class="flex flex-col gap-2 mt-5">
            <label class="font-semibold ">Name:</label>
            <p class="text-rose-800">{{ error.nameErr.toString() }}</p>
            <input
              class="p-2 rounded-lg outline-none"
              type="text"
              placeholder="profile name"
              @keyup="nameValidation()"
              required
              v-model="profile.name"
            />
          </div>
          <div class="flex flex-col gap-2">
            <label class="font-semibold" for="filter">Email:</label>
            <p class="text-rose-800">{{ error.emailErr.toString() }}</p>
            <input
              class="p-2 rounded-lg outline-none"
              type="email"
              placeholder="Enter your Email"
              @keyup="emailValidation()"
              required
              v-model="profile.email"
            />
          </div>
          <div class="flex flex-col gap-2">
            <label class="font-semibold">Specialisation:</label>
            <p class="text-rose-800">
              {{ error.specialisationErr.toString() }}
            </p>
            <div
              class="flex flex-row justify-start gap-2 items-center"
              v-for="(option, index) in specialisation"
              :key="index"
            >
              <input
                type="checkbox"
                v-model="profile.specialisation"
                :true-value="option"
                :value="option"
                name=""
                id=""
              />
              <label class="font-medium text-white">{{ option }}</label>
            </div>
          </div>

          <button
            class="p-2 rounded-lg bg-violet-100 text-violet-500 font-bold"
            @click="addProfile"
          >
            Add
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProfileCard from "./components/ProfileCard";

export default {
  name: "App",

  components: {
    ProfileCard,
  },

  data() {
    return {
      searchText: "",
      profile: {
        name: "",
        email: "",
        specialisation: [],
      },
      specialisation: [
        "Surgeon",
        "Radiologist",
        "Cardiologist",
        "Psychiatrist",
        "Dermatologist",
      ],
      error: {
        nameErr: [],
        emailErr: [],
        specialisationErr: [],
      },

      profiles: [
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34,
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28,
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53,
        },
      ],
    };
  },
  computed: {
    search() {
      return this.profiles.filter((profile) => {
        return profile.name
          .toLowerCase()
          .includes(this.searchText.toLowerCase());
      });
    },
  },
  methods: {
    sortAsc() {
      this.profiles.sort(function (a, b) {
        return a.likes - b.likes;
      });
    },

    sortDesc() {
      this.profiles.sort(function (a, b) {
        return b.likes - a.likes;
      });
    },
    // click button in profileCard component to change likes value
    clickCall(val,id){
      const index = this.profiles.findIndex(profile => profile.id == id)
      this.profiles[index].likes = val
    },
    // reset from after submit
    resetForm() {
      Object.keys(this.profile).forEach((key) => {
        if (key == "specialisation") {
          this.profile.specialisation = [];
        } else this.profile[key] = "";
      });
    },
    addProfile() {
      // checking errors
      if (this.validation() == true) {
        const tempProfile = {
          id: this.profiles.length + 1,
          name: this.profile.name,
          email: this.profile.email,
          description: this.profile.specialisation.toString(),
          likes: 0,
        };
        //add new profile
        this.profiles.push(tempProfile);
        //reset form data
        this.resetForm();
      }
    },
    // specialisation validation for not empty input
    specialisationValidtion() {
      this.error.specialisationErr = [];
      if (this.profile.specialisation.length === 0) {
        this.error.specialisationErr.push("Specialisation is required");
        return false;
      }
    },
    nameValidation() {
      this.error.nameErr = [];
      if (!this.profile.name) {
        this.error.nameErr.push("Name is required");
        return false;
      } else if (/^[a-zA-Z]+$/.test(this.profile.name) == false) {
        this.error.nameErr.push("invalid Name");
        return false;
      } else {
        this.error.nameErr = [];
        return true;
      }
    },
    emailValidation() {
      this.error.emailErr = [];
      if (!this.profile.email) {
        this.error.emailErr.push("Email is required");
        return false;
      } else if (
        /^\w+([\\.-]?\w+)*@\w+([\\.-]?\w+)*(\.\w{2,3})+$/.test(
          this.profile.email
        ) == false
      ) {
        this.error.emailErr.push("Email is inValid");
        return false;
      } else {
        this.error.emailErr = [];
        return true;
      }
    },
    validation() {
      const nameIsValid = this.nameValidation();
      const emaiIslValid = this.emailValidation();
      const specialisationIslValid = this.specialisationValidtion();
      if (
        nameIsValid == false ||
        emaiIslValid == false ||
        specialisationIslValid == false
      ) {
        return false;
      } else {
        return true;
      }
    },
  },
};
</script>
<style>
</style>
