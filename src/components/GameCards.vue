<template>
    <div class="game-area"> 
        <h1 class="title">Which <span>MOOD</span> are you in <strong>?</strong></h1>
        <h4 class="description">Pick one of the open card. Then click the close one.</h4>
        <h4 class="description">...and find out if you're in the right mood. </h4>
        <div class="container">
            <transition-group name="rotate-all" appear class="card-container">
                <app-card
                    :key="card.id"
                    :class="{'shadow' : selectedCard == card.id}"
                    @click.native="selectedCard = card.id"
                    v-for="card in cards" 
                    :card="card ">
                        
                </app-card>

            </transition-group>
            
        </div>
        <div class="container">
            <transition name="rotate" mode="out-in">
                <component
                    @click.native="showCard(answer)"
                    :card="answer"
                    :is="activeCard">
                </component>
            </transition>
        </div>
    </div>



</template>

<script>
    import Card from "./Card";
    import DefaultCard from "./DefaultCard";

    export default {
        components : {
            appCard : Card,
            appDefaultCard : DefaultCard
        },
        data(){
                return {
                    selectedCard: null,
                    answer : {},
                    activeCard : "app-default-card",
                    cards : [
                        { id: 1, component:"app-card", image: "src/assets/card-1.png"},
                        { id: 2, component:"app-card", image: "src/assets/card-2.png"},
                        { id: 3, component:"app-card", image: "src/assets/card-3.png"},
                        { id: 4, component:"app-card", image: "src/assets/card-4.png"},
                        { id: 5, component:"app-card", image: "src/assets/card-5.png"},
                    ]
                }
            },
            created(){
                let answer = Math.ceil(Math.random() * this.cards.length);
                // 1 - 5 arası için;
                this.answer = this.cards[answer-1];

            },
            methods : {
                showCard(answer){
                    if(this.selectedCard == null){
                        alert("Pick one of the card!")
                    }
                    else {
                        this.activeCard = answer.component;
                        setTimeout(()=>{
                          if(answer.id == this.selectedCard) {
                            this.$emit("activeComponentEvent", "app-celebrate");
                        }
                        else {
                            this.$emit("activeComponentEvent", "app-failure");

                        }  
                        },1000)
                        
                    }
                }
            }

    }
    
</script>

<style scoped>
    .title{
        text-align: center;
        color: rosybrown;
    }
    .title span {
        color:mediumpurple;
    }
    .title strong {
        color:rgb(114, 88, 88);
    }
    .description {
        color: rgb(112, 112, 112);
        text-align: center;
    }
    .container, .card-container {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: 30px;
    }
    .shadow {
         box-shadow: 0px 5px 48px #30969f !important;  /* Hover devreye girmesin diye !important kullanıyoruz */
        transition: box-shadow .5s;
    }

    /******* Açık Kartların Animasyonları için Gerekli olan Transition Class Tanımları *******/
    /* Deattach olmadığı için leave kullanmamıza gerek kalmıyor */
    .rotate-all-enter{}
    .rotate-all-enter-active{
        animation: rotate-all ease-in-out 2s forwards;
    }
    .rotate-all-leave{}
    .rotate-all-leave-active{}

@keyframes rotate-all {
    from{
        transform: rotateY(0);
    }
    to {
        transform: rotateY(1080deg)
    }
}

    /******* Kapalı Kartların Animasyonları için Gerekli olan Transition Class Tanımları *******/

    .rotate-enter{}
    .rotate-enter-active{
        animation: rotate-in .5s ease-in-out forwards;
    }
    .rotate-leave{}
    .rotate-leave-active{
        animation: rotate-out .5s ease-in-out forwards;
    }

    @keyframes rotate-in {
        from{
            transform: rotateY(90deg);
        }
        to{
            transform: rotateY(0deg);

        }
    }

    @keyframes rotate-out {
        from{
            transform: rotateY(0deg);
        }
        to{
            transform: rotateY(90deg);

        }
    }


</style>