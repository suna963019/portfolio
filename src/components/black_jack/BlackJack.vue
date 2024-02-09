<template>
    <div class="game_box">
        <div class="card_box">
            <Card v-for="data in dearer_card" :card-data="data" />
        </div>
        <div id="point_box">
            <p>{{ this.point }} point</p>
        </div>
        <div id="next_box">
            <button v-if="next" id="next" @click="set()">next</button>
            <p id="check_win">{{ check_win.text }}</p>
        </div>
        <div id="button_box">
            <button id="double" @click="double()">Double</button>
            <button id="hit" @click="hit()">Hit</button>
            <button id="stand" @click="stand()">Stand</button>
        </div>
        <div class="card_box">
            <Card v-for="data in my_card" :card-data="data" />
        </div>
    </div>
</template>
<script>
import Card from './Card.vue';
export default {
    components: {
        Card
    },
    data() {
        return {
            cardList: [],
            cardCheck: [],
            cardKinds: 0,
            cardNum: 0,
            myCardNum: 0,
            dearerCardNum: 0,
            playerCardsPointArray: [],
            dearerCardsPointArray: [],
            moveCheck: true,
            point: 100,
            pointReturn: 0,
            my_card: [],
            dearer_card: [],
            check_win: { text: '', color: 'none' },
            next: false,
        }
    },
    mounted() {
        this.set()
    },
    methods: {
        //初期化
        set() {
            this.pointReturn = 10;
            this.cardList = [];
            this.cardCheck = [];
            this.cardKinds = 0;
            this.cardNum;
            for (let i = 1; i < 53; i++) {
                if ((i - 1) % 13 === 0) {
                    this.cardKinds++;
                }
                this.cardNum = i % 13 + 1;
                this.cardList.push([this.cardKinds, this.cardNum]);
                this.cardCheck.push(0);
            }
            this.myCardNum = 0;
            this.dearerCardNum = 0;
            this.playerCardsPointArray = [];
            this.dearerCardsPointArray = [];
            this.moveCheck = true;
            this.my_card = [];
            this.dearer_card = [];
            this.check_win = [];
            this.next = false
            this.start();
        },
        //ボタン処理
        double() {
            if (this.moveCheck) {
                this.pointReturn = 20;
                if (this.HandOutCard()) {
                    this.dearerStart();
                }
            }
        },
        hit() {
            if (this.moveCheck) {
                this.HandOutCard();
            }
        },
        stand() {
            if (this.moveCheck) {
                this.dearerStart();
            }
        },
        start() {
            this.dearerHandOutCards();
            this.HandOutCard();
            this.dearerHandOutCards();
            this.HandOutCard();
        },
        end() {
            this.deckCheck();
            this.next = true;
        },
        //ポイントが超えてないかチェック
        checkOutPoints(pointList) {
            let allPoint = 0;
            let allPoint2 = 0;
            let allPointCheck = false;
            let allPoint2Check = false;
            let checkA = true;
            for (const i of pointList) {
                allPoint += i;
                allPoint2 += i;
                if (i === 1 && checkA) {
                    allPoint2 += 10;
                    checkA = false;
                }
            }
            if (allPoint > 21) {
                allPointCheck = true;
            }
            if (allPoint2 > 21) {
                allPoint2Check = true;
            }
            return [allPointCheck, allPoint, allPoint2Check, allPoint2];
        },
        //プレイヤーがカードをもらう
        HandOutCard() {
            let myCardArray = this.randomSelect();
            let myCardKind = this.cardKindsChange(myCardArray[0]);
            this.my_card.push({ num: myCardArray[1], type: myCardKind[0], color: myCardKind[1] });
            let playerPlusPoint = myCardArray[1];
            if (playerPlusPoint > 10) {
                playerPlusPoint = 10;
            }
            this.playerCardsPointArray.push(playerPlusPoint);
            if (this.checkOutPoints(this.playerCardsPointArray)[0]) {
                this.moveCheck = false;
                this.winPrint(false);
                return false
            }
            return true
        },
        //カードのタイプの置き換え
        cardKindsChange(num) {
            let cardKinds;
            let cardColor;
            switch (num) {
                case 1:
                    cardKinds = "♥";
                    cardColor = "red";
                    break;
                case 2:
                    cardKinds = "♦";
                    cardColor = "red";
                    break;
                case 3:
                    cardKinds = "♠";
                    cardColor = "black";
                    break;
                case 4:
                    cardKinds = "♣";
                    cardColor = "black";
                    break;
                default:
            }
            return [cardKinds, cardColor];
        },
        //カードのランダム選択
        randomSelect() {
            let cardSelectIndex;
            let cardSelect;
            while (true) {
                cardSelectIndex = Math.floor(Math.random() * 52);
                cardSelect = this.cardList[cardSelectIndex];
                if (this.cardCheck[cardSelectIndex] === 0) {
                    this.cardCheck[cardSelectIndex] = 1;
                    break;
                }
            }
            return cardSelect;
        },
        //ディーラーのターンの処理
        dearerStart() {
            let dearerPointCheckArray;
            let playerPointCheckArray;
            while (true) {
                dearerPointCheckArray = this.checkOutPoints(this.dearerCardsPointArray);
                playerPointCheckArray = this.checkOutPoints(this.playerCardsPointArray);
                if (this.pointArrayCheck()) {
                    break;
                }
                if (dearerPointCheckArray[3] >= 17 && dearerPointCheckArray[3] <= 21) {
                    break;
                } else if (dearerPointCheckArray[1] > 17) {
                    break;
                } else {
                    this.dearerHandOutCards();
                }
            }

            if (dearerPointCheckArray[0]) {
                this.winPrint(true);
            } else {
                this.winCheck();
            }

        },
        //勝敗確認
        pointArrayCheck() {
            let dearerPointCheckArray = this.checkOutPoints(this.dearerCardsPointArray);
            let playerPointCheckArray = this.checkOutPoints(this.playerCardsPointArray);
            let playerPointCheck = playerPointCheckArray[1];
            let dearerPointCheck = dearerPointCheckArray[1];
            if (!playerPointCheckArray[2]) {
                playerPointCheck = playerPointCheckArray[3];
            }
            if (!dearerPointCheckArray[2]) {
                dearerPointCheck = dearerPointCheckArray[3];
            }
            if (playerPointCheck < dearerPointCheck) {
                return true;
            } else {
                return false;
            }

        },
        //ディーラーがカードをもらう
        dearerHandOutCards() {
            let dearerCardArray = this.randomSelect();
            let dearerCardKind = this.cardKindsChange(dearerCardArray[0]);
            this.dearer_card.push({ num: dearerCardArray[1], type: dearerCardKind[0], color: dearerCardKind[1] })
            let dearerPlusPoint = dearerCardArray[1];
            if (dearerPlusPoint > 10) {
                dearerPlusPoint = 10;
            }
            this.dearerCardsPointArray.push(dearerPlusPoint);
        },
        //勝敗確認後の処理
        winCheck(playerPoint) {
            let playerPointCheckArray = this.checkOutPoints(this.playerCardsPointArray);
            let playerPointCheck;
            if (!playerPointCheckArray[2]) {
                playerPointCheck = playerPointCheckArray[3];
            } else if (!playerPointCheckArray[0]) {
                playerPointCheck = playerPointCheckArray[1];
            } else {
                playerPointCheck = 0;
            }

            let dearerPointCheckArray = this.checkOutPoints(this.dearerCardsPointArray);
            let dearerPointCheck;
            if (!dearerPointCheckArray[2]) {
                dearerPointCheck = dearerPointCheckArray[3];
            } else if (!dearerPointCheckArray[0]) {
                dearerPointCheckArray = dearerPointCheckArray[1];
            } else {
                dearerPointCheck = 1;
            }
            if (playerPointCheck > dearerPointCheck) {
                this.winPrint(true);
            } else if (playerPointCheck < dearerPointCheck) {
                this.winPrint(false);
            } else {
                this.check_win.text = "draw";
                this.check_win.color = "gray";
                this.end();
            }


        },
        //決着後処理
        winPrint(winnerCheck) {
            this.moveCheck = false;
            if (winnerCheck) {
                this.check_win.text = "Win";
                this.check_win.color = "red";
                this.point += this.pointReturn;
            } else if (!winnerCheck) {
                this.check_win.text = "Lose";
                this.check_win.color = "blue";
                this.point -= this.pointReturn;
            }
            this.end();
        },
        //デッキの確認
        deckCheck() {
            let checkUsedCount = 0;
            for (const i of this.cardCheck) {
                if (i === 0) {

                    break;
                }
            }
            if (checkUsedCount > 26) {
                this.deckReset();
            }
        },
        //デッキの初期化
        deckReset() {
            for (const i of this.cardCheck) {
                i = 0;
            }
        }
    },
}
</script>
<style scoped>
.game_box {
    height: 800px;
    width: 1200px;
    margin: auto;
    padding: 30px 60px;
    background-color: green;
    border: 8px solid greenyellow;
    border-radius: 10px;
}

.card_box {
    display: flex;
}

#point_box {
    height: 80px;
    display: flex;
    justify-content: center;
    font-size: 30px;
    color: whitesmoke;
}

#next_box {
    display: flex;
    height: 70px;
}

#check_win {
    margin-left: 400px;
    text-align: center;
    font-size: 50px;
    color: v-bind(check_win.color);
}

#button_box {
    display: flex;
    justify-content: center;
}

button {
    padding: 0 5px;
    margin: 10px;
    font-size: 30px;
    border: 3px solid whitesmoke;
    border-radius: 15px;
    color: whitesmoke;
}
</style>