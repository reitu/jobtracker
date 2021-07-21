<template>
  <div class="nav">👩🏽‍💻 JOB TRACKER</div>

  <div class="add-sec">
    <div>
      <input
        v-model="title"
        class="input"
        type="text"
        name="title"
        id="title"
        placeholder="Job title"
      />

      <input
        v-model="location"
        class="input"
        type="text"
        id="location"
        placeholder="Job location"
      />

      <input
        v-model="description"
        class="input"
        type="text"
        id="description"
        placeholder="Job description"
      />

      <input
        v-model="link"
        class="input"
        type="text"
        id="link"
        placeholder="Job link"
      />
    </div>

    <div>
      <button @click="addJobs" class="input" id="btnadd">ADD JOB</button>
    </div>
    <p style="color: grey">Fill in all fields*</p>
  </div>
  <div class="show-sec">
    <div v-for="(item, i) in jobs" :key="i">
      <div v-if="!item.applied && item.title && item.description && item.location && item.link" class="container" id="notapplied">
        <div>
          <p>👩🏽‍💻 {{ item.title }}</p>
          <p>📍 {{ item.location }}</p>
          <a>🔗 {{ item.link }}</a>
          <p>📝 {{ item.description }}</p>
        </div>
        <div class="btns">
          <button @click="toggleApplied(item)" id="btnApplied">Applied</button>

          <button @click="dltJob(item)" id="btnRemove">Delete</button>
        </div>
      </div>

      <div v-if="item.applied && item.title && item.description && item.location && item.link" class="container" id="applied">
        <div>
          <span style="background: yellow"> APPLIED </span>
          <p>👩🏽‍💻 {{ item.title }}</p>
          <p>📍 {{ item.location }}</p>
          <a>🔗 {{ item.link }}</a>
          <p>📝 {{ item.description }}</p>
        </div>
        <div class="btns">
          <button @click="toggleApplied(item)" id="btnApplied">Undo</button>

          <button @click="dltJob(item)" id="btnRemove">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      jobs: this.getJobs(), //we are initializing jobs to whaetever is in the local storage
      title: "",
      location: "",
      description: "",
      link: "",
    };
  },

  watch: {
    //it watches for changes in our data; and when we see changes the handler follows the intruction given
    jobs: {
      //and in this case it is to push jobs if jobs is changed in any way; (so no need for us to manually push like before since this handles it)
      handler() {
        this.pushJobs();
      },
      deep: true, //watches everything inside the array
    },
  },

  methods: {
    addJobs() {
      this.jobs.push({
        title: this.title,
        location: this.location,
        description: this.description,
        link: this.link,
        applied: false,
      });
      this.title = "";
      this.location = "";
      this.description = "";
      this.link = "";
    },

    pushJobs() {
      var myJSONstring = JSON.stringify(this.jobs);
      window.localStorage.setItem("allJobs", myJSONstring);
    },

    getJobs() {
      var storedStr = window.localStorage.allJobs;
      if (storedStr) {
        return JSON.parse(storedStr);
      }
      if (!storedStr) {
        return [];
      }
    },

    toggleApplied(theJob) {
      theJob.applied = !theJob.applied; //DEEP = goes into the array, into the job object, into the applied property, into the booolean value and its like oh that changed, and saves the latest value to the local storage.
    },

    dltJob(theJob) {
      //the watcher watches for changes ONLY. (takes the way the jobs looks like now and syncs that) and SAVES THE LATEST COPY TO LOCAL STORAGE!!!
      this.jobs = this.jobs.filter(function (item) {
        return item !== theJob;
      });
    },
  },
};
</script>

<style scoped>
html * {
  font-size: 12px;
  font-family: "Poppins", sans-serif;
}

.nav {
  box-shadow: 0 4px 2px 0 rgb(100 121 143 / 12%);
  color: rgb(90, 86, 86);
  margin: 0;
  min-width: 100%;
  height: 40px;
  font-size: 24px;
  padding: 16px;
  align-items: center;
}

.add-sec {
  margin: 60px;
  padding: 20px;
  box-shadow: 0 4px 2px 0 rgb(100 121 143 / 12%);
  background-color: #eff2f5;
  border-radius: 8px;
  text-align: center;
}

.add-sec .input {
  width: 200px;
  height: 24px;
  box-sizing: 4px;
  margin: 8px;
}

.show-sec {
  margin: 60px;
  border-radius: 8px;

  overflow-y: scroll;
  height: 600px;
  box-shadow: 0 4px 2px 0 rgb(100 121 143 / 12%);
  background-color: #eff2f5;
}

.container {
  background-color: white;
  box-shadow: 2px 2px rgb(219, 213, 213);
  border-radius: 8px;
  margin: 16px;
  height: 160px;
  display: flex;
  justify-content: space-between;
  padding: 12px;
  font-weight: 300;
}

#applied {
  background-color: white;
  font-size: 10px;
}

#btnadd {
  padding: 4px;
  margin: 8px;
  width: 100px;
  height: 40px;
  color: white;
  font-weight: bolder;
  background-color: #1a5cff;
  border: solid 2px #1a5cff;
  border-radius: 8px;
}

#btnApplied {
  padding: 4px;
  margin: 8px;
  width: 100px;
  height: 40px;
  color: white;
  font-weight: bolder;
  background-color: #1a44ff;
  border: solid 2px #1a5cff;
  border-radius: 8px;
}

#btnRemove {
  padding: 4px;
  margin: 8px;
  width: 100px;
  height: 40px;
  color: white;
  font-weight: bolder;
  background-color: #ff0000;
  border: solid 2px #ff0000;
  border-radius: 8px;
}
</style>



                    




