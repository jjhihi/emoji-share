<template>
<v-card class="mx-auto test-width" >
    <v-toolbar
      color="indigo"
      dark
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Emoji List</v-toolbar-title>

      <v-spacer></v-spacer>

<v-text-field hide-details="auto" prepend-icon="mdi-magnify" :value="keyword" @change="setKeyword">
</v-text-field>

      <v-btn-toggle mandatory group @change="changeListType" v-model="listType">
          <v-btn icon value="list">
            <v-icon>mdi-format-list-bulleted-square</v-icon>
          </v-btn>
          <v-btn icon value="grid">
            <v-icon>mdi-view-grid</v-icon>
          </v-btn>
        </v-btn-toggle>
    </v-toolbar>
<v-navigation-drawer
      v-model="drawer"
      absolute
      temporary
    >
      <v-list
        nav
        dense
      >
        <v-list-item-group
          active-class="deep-purple--text text--accent-4"
        >
          <v-list-item>
            <v-list-item-title>Foo</v-list-item-title>
          </v-list-item>

          <v-list-item>
            <v-list-item-title>Bar</v-list-item-title>
          </v-list-item>

          <v-list-item>
            <v-list-item-title>Fizz</v-list-item-title>
          </v-list-item>

          <v-list-item>
            <v-list-item-title>Buzz</v-list-item-title>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <v-container fluid>
      <v-row dense v-if="listType == 'grid'">
        <v-col
          v-for="(card, index) in cards"
          :key="index"
          :cols="6"
          xs="6"
          sm="4"
          md="3"
          lg="3"
          xl="3">
          <v-card>
            <v-img
              :src="baseUrl+card['thumbnail-iamge']"
              class="white--text align-end"
              gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)">
              <v-card-title v-text="card.description"></v-card-title>
            </v-img>

            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn icon @click="showExpand(index)" >
                 <v-icon>{{ show[index] ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
              </v-btn>
              <v-btn icon @click="callAppShare(index)">
                <v-icon>mdi-share-variant</v-icon>
              </v-btn>
              <v-btn icon @click="downloadEmoji(card['emoji-file'])">
                <v-icon>mdi-download</v-icon>
              </v-btn>
            </v-card-actions>

            <v-expand-transition>
               <div v-show="show[index]">
                  <v-divider></v-divider>

                  <v-card-text>
                emoji 다른 사진을 여기에 보여주자.
                  </v-card-text>
               </div>
            </v-expand-transition>
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
            <v-list-item-subtitle>
                emoji 다른 사진을 여기에 보여주자.
            </v-list-item-subtitle>
          </v-list-item-content>

          <v-list-item-action>
              <v-btn icon @click="callAppShare(index)">
                <v-icon>mdi-share-variant</v-icon>
              </v-btn>
            <v-btn icon @click="downloadEmoji(card['emoji-file'])">
              <v-icon color="grey lighten-1">mdi-download</v-icon>
            </v-btn>
          </v-list-item-action>
        </v-list-item>
      </template>
    </v-list>
<div class="text-center">
    <v-pagination
      v-model="page"
      :length="15"
      :total-visible="7"
      circle
      @input="setPage"
    ></v-pagination>
  </div>
<v-fab-transition>
        <v-btn
            v-scroll="onScroll"
            v-show="fab"
            fab
            fixed
            bottom
            right
color="primary"
            @click="toTop"
          >
        <v-icon>mdi-chevron-double-up</v-icon>
          </v-btn>
</v-fab-transition>
      
    </v-container>
</v-card>
</template>

<script>
  export default {
    name: 'EmojiList',
    data: () => ({
fab:false,
      page: 1,
      keyword: "",
      drawer: false,
      cards: [], 
      show: [],
      listType: undefined,
      //baseUrl: "https://raw.githubusercontent.com/jjhihi/emoji-share/main/public/sample_data/",
      baseUrl: "/sample_data/",
    }),
    methods: {
onScroll (e) {
      if (typeof window === 'undefined') return
      const top = window.pageYOffset ||   e.target.scrollTop || 0
      this.fab = top > 20
    },
    toTop () {
      this.$vuetify.goTo(0)
    },
      setKeyword(keyword) {
         console.log(keyword);
      },
      setPage( page ) {
         this.page = page;
      },
      callAppShare( index ) {
        //console.log(index, this.cards[index]);
        //window.Android.doShare( arg1, arg2, arg3 ); 
      },
      changeListType(key) {
        this.listType = key;
      },
      showExpand(index) {
        if(this.show[index]){
           this.show = new Array(this.show.length).fill(false);
        }else{
           this.show = new Array(this.show.length).fill(false).fill(true, index, index+1);
        }
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
         self.cards= response.data;
         self.show = new Array(response.data.length).fill(false);
         }).catch(function(error) {
           alert(error);
         });
    },
    created() {
       let self = this; 
       self.listType = "grid";
    },
    updated() {
//console.log("updated", this.show);
    }
  }
</script>
