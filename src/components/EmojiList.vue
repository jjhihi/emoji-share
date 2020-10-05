<template>
    <v-container fluid>
      <v-row justify="end">
      <v-btn-toggle mandatory @change="changeListType" v-model="listType">
          <v-btn value="list">
            <v-icon>mdi-format-list-bulleted-square</v-icon>
          </v-btn>
          <v-btn value="grid">
            <v-icon>mdi-view-grid</v-icon>
          </v-btn>
        </v-btn-toggle>
      </v-row>
      <v-row dense v-if="listType == 'grid'">
        <v-col
          v-for="(card, index) in cards"
          :key="index"
          :cols="6"
        >
          <v-card>
            <v-img
              :src="baseUrl+card['thumbnail-iamge']"
              class="white--text align-end"
              gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
              height="200px"
            >
              <v-card-title v-text="card.description"></v-card-title>
            </v-img>

            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn icon @click="downloadEmoji(card['emoji-file'])">
                <v-icon>mdi-download</v-icon>
              </v-btn>

            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>


      <v-list three-line v-else-if="listType == 'list'">
      <template v-for="(card, index) in cards">
        <v-divider
          v-if="index > 0"
          :key="'d'+index"
        ></v-divider>
        <v-list-item :key="'l'+index" >
          <v-list-item-avatar>
            <v-img :src="baseUrl+card['thumbnail-iamge']"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title v-html="card.description"></v-list-item-title>
            <v-list-item-subtitle v-html="card.description"></v-list-item-subtitle>
          </v-list-item-content>

          <v-list-item-action>
            <v-btn icon @click="downloadEmoji(card['emoji-file'])">
              <v-icon color="grey lighten-1">mdi-download</v-icon>
            </v-btn>
          </v-list-item-action>
        </v-list-item>
      </template>
    </v-list>
      
    </v-container>
</template>

<script>
  export default {
    name: 'EmojiList',
    data: () => ({
      cards: [], 
      listType: undefined,
      //baseUrl: "https://raw.githubusercontent.com/jjhihi/emoji-share/main/public/sample_data/",
      baseUrl: "/sample_data/",
    }),
    methods: {
      changeListType(key) {
        this.listType = key;
      },
      downloadEmoji(path) {
        let url = this.baseUrl + path;

        this.$http({url: url, method: 'GET', responseType: 'blob', }).then((response) => {
             let fileURL = window.URL.createObjectURL(new Blob([response.data]));
             let fileLink = document.createElement('a');
          
             fileLink.href = fileURL;
             fileLink.setAttribute('download', path);
             document.body.appendChild(fileLink);
             fileLink.click();
        });
      }

    },
    mounted() {
    },
    beforeCreate(){
       let self = this; 
       //this.$http.get("https://raw.githubusercontent.com/jjhihi/emoji-share/main/public/sample_data/list.json").then(function(response) { 
       this.$http.get("/sample_data/list.json").then(function(response) { 
         self.cards= response.data;}
         ).catch(function(error) {
           alert(error);
         });
    },
    created() {
       let self = this; 
       self.listType = "grid";
    },
    updated() {
    }
  }
</script>
