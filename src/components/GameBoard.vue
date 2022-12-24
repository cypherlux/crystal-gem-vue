<!-- eslint-disable prettier/prettier -->
<script>
    import CardView from "./CardView.vue";
    import cardData from "../data/memoryCards8.js";

    export default {
        components: {
            CardView,
        },
        data() {
            return {
                cardsData: cardData.concat(cardData),
                matchedPairs: 0,
                cardOne: "",
                cardTwo: "",
                disableDeck: false,
            };
        },
        mounted() {
            this.shuffleCards();
        },
        methods: {
            shuffleCards() {
                this.matchedPairs = 0;
                this.disableDeck = false;
                this.cardOne = "";
                this.cardTwo = "";
                this.cardsData = this.cardsData.sort(function () {
                    return 0.5 - Math.random();
                });
            },
            flipCard(evt) {
                const clickedCard = evt.target; 
                if (this.cardOne !== clickedCard && !this.disableDeck) { 
                    clickedCard.classList.add("flip"); 
                    if (!this.cardOne) {
                        return (this.cardOne = clickedCard);
                    }
                    this.cardTwo = clickedCard; 
                    this.disableDeck = true; 
                    let cardOneImg = this.cardOne.querySelector(".back-view img").src; 
                    let cardTwoImg = this.cardTwo.querySelector(".back-view img").src; 
                    matchCards(cardOneImg, cardTwoImg);
                }
            },
            matchCards(img1, img2) {
                if (img1 === img2) {
                    matchedPairs++;
                    if (matchedPairs == 8) {
                        console.log('YOU WIN :D');
                        setTimeout(function(){
                            alert("You win!!"); 
                }, 1000);
                        return;
                    }
                    cardOne.removeEventListener("click", flipCard);
                    cardTwo.removeEventListener("click", flipCard);
                    cardOne = cardTwo = "";
                    disableDeck = false;
                    return;
                }
                setTimeout(() => {
                    cardOne.classList.add("shake");
                    cardTwo.classList.add("shake");
                }, 400);
                setTimeout(() => {
                    cardOne.classList.remove("shake", "flip");
                    cardTwo.classList.remove("shake", "flip");
                    cardOne = cardTwo = "";
                    disableDeck = false;
                    return;
                }, 1200);
            }
        },
    };
</script>

<template>
    <div class="game-board">
        <ul class ="cards">
            <li 
                v-for="(cardInfo, index) in cardsData" 
                :key="index" 
                class="card"
                @click="flipCard"
            >
                <CardView viewType="front"/>
                <CardView
                    viewType="back"
                    :imageUrl="cardInfo.url"
                    :imageAltText="cardInfo.altText"
                />
            </li>
        </ul>
    </div>
</template>