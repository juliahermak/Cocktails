<template>
  <AppLeyout
    imgUrl="/src/assets/img/bg-1.png"
    :backFunc="removeIngredient"
    :is-back-btn-visible="!!ingredient"
  >
    <div class="wrapper">
      <div v-if="!ingredient || !cocktails" class="info">
        <div class="title">Choose your drink</div>
        <div class="line"></div>
        <div class="select-wrapper">
          <el-select
            v-model="rootStore.ingredient"
            filterable
            allow-create
            class="select"
            placeholder="Choose main ingredient"
            size="large"
            :teleported="false"
            @change="getCocktails"
          >
            <el-option
              v-for="item in ingredients"
              :key="item.strIngredient1"
              :label="item.strIngredient1"
              :value="item.strIngredient1"
            />
          </el-select>
        </div>
        <div class="text">
          Try our delicious cocktail recipes for every occasion. Find delicious
          cocktail recipes by ingredient through our cocktail generator.
        </div>
        <img src="/src/assets/img/cocktails.png" class="img" />
      </div>
      <div v-else class="info">
        <div class="title">COCKTAILS WITH {{ ingredient }}</div>
        <div class="line"></div>
        <div class="cocktails">
          <CocktailThumb
            v-for="cocktail in cocktails"
            :key="cocktail.idDrink"
            :cocktail="cocktail"
          ></CocktailThumb>
        </div>
      </div>
    </div>
  </AppLeyout>
</template>
<script setup>
import { ref } from "vue";
import AppLeyout from "../components/AppLeyout.vue";
import CocktailThumb from "@/components/CocktailThumb.vue";
import { useRootStore } from "@/stores/root";
import { storeToRefs } from "pinia";

const rootStore = useRootStore();
rootStore.getIngredients();

const { ingredients, ingredient, cocktails } = storeToRefs(rootStore);

function getCocktails() {
  rootStore.getCocktails(rootStore.ingredient);
}

function removeIngredient() {
  rootStore.setIngredient(null);
}
</script>
<style lang="sass" scoped>
@import "../assets/styles/main"


.select-wrapper
  padding-top: 50px


.select
  width: 220px

.text
  width: 516px
  margin: 0 auto
  padding-top: 50px
  line-height: 36px
  letter-spacing: 0.1em
  color: $textMuted

.img
   margin-top: 60px

.cocktails
  display: flex
  flex-wrap: wrap

  max-height: 400px
  overflow-y: auto
  align-items: center
  margin-top: 90px
</style>
