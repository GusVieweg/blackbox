<template>
  <v-container class="py-0 mt-6">
    <v-row align="center" class="pb-5">
      <!-- APP TITLE BLOCK -->
      <v-col>
        <h1>
          <v-icon color="black" justify="center">mdi-checkbox-blank</v-icon
          >Blackbox
        </h1>
      </v-col>
    </v-row>
    <v-row class="py-0 my-0">
      <!-- TITLE ENTRY BOX -->
      <v-col cols="3">
        <v-text-field
          class="py-0 my-0"
          type="text"
          name="title"
          label="Puzzle Title"
          hide-details
          autocomplete="off"
          dense
          single-line
        ></v-text-field>
      </v-col>
      <v-col cols="1">
        <b>by</b>
      </v-col>
      <v-col cols="2">
        <v-text-field
          class="py-0 my-0"
          type="text"
          name="author"
          label="Your Name Here"
          autocomplete="off"
          dense
          single-line
        ></v-text-field>
      </v-col>
      <v-col cols="1"></v-col>
      <v-col cols="1">
        <v-btn-toggle>
          <v-btn small depressed block text color="#5F917A">
            <v-icon>mdi-checkbox-blank</v-icon>
          </v-btn>
        </v-btn-toggle>
      </v-col>
      <v-col cols="1">
        <v-select items="5x5" label="Size" dense single-line> </v-select>
      </v-col>
    </v-row>
    <v-row class="mt-6">
      <v-col cols="8" class="mr-0"
        ><div style="width:700px; height:600px; border:1px solid #000">
          <h2>Grid goes here!</h2>
        </div></v-col
      >
      <v-col cols="1" class="ml-0">
        <div style="width:300px; height: 600px; border: 1px solid #000">
          <h2>
            Word Lookup goes here!
          </h2>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {};
</script>

<style></style>
