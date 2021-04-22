<template>
<div class="AddEdit">
  <h1>Add photography to the gallery</h1>
    <div class="heading">
      <h2>Add Photography</h2>
    </div>
    <div class="add">
      <div class="form">
        <input v-model="title" placeholder="Title">
        <p></p>
        <input type="file" name="photo" @change="fileChanged">
        <p><textarea v-model="description" placeholder="Describe the picture"></textarea></p>
        <p><textarea v-model="place" placeholder="Where did you capture the moment?"></textarea></p>
        <button @click="upload">Upload</button>
      </div>
      <div class="upload" v-if="addItem">
        <h2>{{addItem.title}}</h2>
        <img :src="addItem.path" />
        <p>{{addItem.description}}</p>
        <p>-{{addItem.place}}-</p>
      </div>
    </div>
    <!-- add delete -->
        <div class="heading">
      <h2>Edit/Delete a photography</h2>
    </div>
    <div class="edit">
      <div class="form">
        <input v-model="findTitle" placeholder="Search">
        <div class="suggestions" v-if="suggestions.length > 0">
          <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
          </div>
        </div>
      </div>
      <div class="upload" v-if="findItem">
        <input v-model="findItem.title">
        <p><textarea v-model="findItem.description" placeholder="Describe this picture..."></textarea></p>
        <p><textarea v-model="findItem.place" placeholder="Where did you capture the moment?"></textarea></p>
        <img :src="findItem.path" />
      </div>
      <div class="actions" v-if="findItem">
        <button @click="deleteItem(findItem)">Delete</button>
        <button @click="editItem(findItem)">Edit</button>
      </div>
    </div>
</div>
</template>

<script>
import axios from "axios";
export default {
  name: "AddEdit",
  data() {
    return {
      title: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
      description: "",
      place: ""
    };
  },
  created() {
    this.getItems();
  },
  computed: {
    suggestions() {
      let items = this.items.filter((item) => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    },
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0];
    },
    async upload() {
      try {
        const formData = new FormData();
        formData.append("photo", this.file, this.file.name);
        let r1 = await axios.post("/api/photos", formData);
        let r2 = await axios.post("/api/items", {
          title: this.title,
          description: this.description,
          place: this.place,
          path: r1.data.path,
        });
        console.log(r2);
        this.addItem = r2.data;
        this.getItems();
      } catch (error) {
        (error) => {error;}
      }
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        (error) => {error;}  
      }
    },
    selectItem(item) {
      this.findTitle = "";
      this.findItem = item;
    },
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        let index = this.items.indexOf(item);
        this.items.splice(index, index + 1);
        this.getItems();
        return true;
      } catch (error) {
        (error) => {error;}
      }
    },
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          description: this.findItem.description,
          place: this.findItem.place
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        (error) => {error;}
      }
    },
  },
};
</script>

<style scoped>

.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}

button {
  background-color: #008CBA;
}

</style>
