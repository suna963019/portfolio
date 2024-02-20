<template>
    <div class="home_box">
        <h1>ゲーム集</h1>
        <p>いくつかのゲームをプレイしそれぞれのランキングに乗せられます。</p>
        <p>※開始時にニックネームを決められ、ランキングではその名前が使われます。　ゲーム画面がはみ出す場合、ブラウザのズーム倍率を変えてください。</p>
        <div class="card_box">
            <div class="card">
                <router-link to="/tetris" class="normal">
                    <div class="row">
                        <h2>テトリス</h2>
                        <p>ロシア発のあのゲームです。</p>
                    </div>
                    <img src="@/assets/tetris_image.png" alt="" class="game_img">
                </router-link>
            </div>
            <div class="card">
                <router-link to="/slot" class="normal">
                    <div class="row">
                        <h2>スロット</h2>
                        <p>簡易的なスロットが遊べます。</p>
                    </div>
                    <img src="@/assets/slot_image.png" alt="" class="game_img">
                </router-link>
            </div>
            <div class="card">
                <router-link to="/typing" class="normal">
                    <div class="row">
                        <h2>タイピング</h2>
                        <p>タイピングゲームです。</p>
                    </div>
                    <img src="@/assets/typing_image.png" alt="" class="game_img">
                </router-link>
            </div>
            <div class="card">
                <router-link to="/blockbreaker" class="normal">
                    <div class="row">
                        <h2>ブロックくずし</h2>
                        <p>ブロックを壊そう。</p>
                    </div>
                    <img src="@/assets/block_breaker_image.png" alt="" class="game_img">
                </router-link>
            </div>
            <div class="card">
                <router-link to="/blackjack" class="normal">
                    <div class="row">
                        <h2>ブラックジャック</h2>
                        <p>幸運を祈る。</p>
                    </div>
                    <img src="@/assets/blackjack_image.png" alt="" class="game_img">
                </router-link>
            </div>
        </div>
    </div>
</template>
<script>
export default {

}
</script>
<style scoped>
h2{
    margin: 0;
}
.home_box {
    color: green;
}

.card {
    border-radius: 30px;
    margin: 50px;
    padding: 20px 30px;
    width: 450px;
    border: 4px solid green;
}

.card_box {
    display: flex;
    flex-wrap: wrap;
}

.normal {
    color: green;
    text-decoration: none;
}

.row {
    display: flex;
    justify-content: space-between;
    text-decoration: none;
    line-height: 48px;
    color: green;
}

.game_img {
    border: 1px solid green;
    width: 100%;
}
</style>