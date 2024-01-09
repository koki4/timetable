<template>
  <v-app>
    <!-- ヘッダー -->
    <v-app-bar app dark clipped-left>
      <v-app-bar-nav-icon @click.stop="drawer=!drawer"></v-app-bar-nav-icon>
      <span class="text-h6">~My Portfolio~</span>
    </v-app-bar>

    <!-- ドロワー -->
    <v-navigation-drawer app v-model="drawer" clipped>
      <v-list>
        <v-list-item-group>
          <v-list-item v-for="item, key in items" :key="key" @click="drawer=!drawer; update(key)">{{item["name"]}}</v-list-item>
        </v-list-item-group>
      </v-list>
    </v-navigation-drawer>

    <!-- メイン -->
    <v-main>
      <v-container>
        <h1>ようこそ</h1>
        <TimeTable v-if="items.timetable.show"></TimeTable>
        
      </v-container>
    </v-main>

    <!-- フッター -->
    <v-footer color="cyan">
    </v-footer>
  </v-app>
</template>

<script>
import TimeTable from './components/TimeTable';


export default {
  name: 'App',

  components: {
    TimeTable,
  },

  data: () => ({
    items: {"timetable": {"name":"時間割","show":false}, "sonota": {"name":"その他", "show": false}},
    drawer: false,
  }),
  methods:{
    update(clicked_key) {
      for(var key in this.items) {
        console.log(this.items[key]["name"], clicked_key)
        if(key == clicked_key) {
          this.items[key]["show"] = true;
        } else {
          this.items[key]["show"] = false;
        }
      }
    },
  }
};
</script>