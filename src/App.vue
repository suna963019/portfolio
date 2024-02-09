<template>
  <v-app>
    <v-main>
      <v-app-bar class="bg-light-green light-green-darken-3">
        <v-app-bar-title>
          <router-link to="/" class="normal bar-title text-white">
            ゲーム集
          </router-link>
        </v-app-bar-title>
        <div id="home-links">
          <!-- v-tabs実装予定 -->
          <router-link to="/" class="normal text-white">
            <v-btn>ホーム</v-btn>
          </router-link>
          <router-link to="/tetris" class="normal text-white">
            <v-btn>テトリス</v-btn>
          </router-link>
          <router-link to="/slot" class="normal text-white">
            <v-btn>スロット</v-btn>
          </router-link>
          <router-link to="/typing" class="normal text-white">
            <v-btn>タイピング</v-btn>
          </router-link>
          <router-link to="/blockbreaker" class="normal text-white">
            <v-btn>ブロックくずし</v-btn>
          </router-link>
          <router-link to="/blackjack" class="normal text-white">
            <v-btn>ブラックジャック</v-btn>
          </router-link>
          <router-link to="/ranking" class="normal text-white">
            <v-btn>ランキング</v-btn>
          </router-link>
        </div>
      </v-app-bar>
      <v-container>
        <router-view id="view"></router-view>
      </v-container>
    </v-main>
  </v-app>
</template>
<script>
export default {

}
</script>
<style scoped>
a,
p {
  color: #000;
}

.normal {
  text-decoration: none;
}
.bar-title{
  margin-left: 30px;
  font-size: 30px;
}
#home-links button {
  font-size: 20px;
  margin: 0 10px;
}
main{
  background-color: rgb(223, 243, 193);
}
</style>