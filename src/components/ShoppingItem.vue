<template>
  <q-item v-if="val === false">
    <div class="q-gutter-md">
      <q-checkbox v-model="val" />
    </div>

    <q-item-section class="q-pl-md">
      <q-item-label class="text-weight-medium">{{nameItem}}</q-item-label>
    </q-item-section>

    <q-item-section side>
      <q-input class="input_style" dense rounded standout type="number" v-model="numberItemChild" @keyup="$emit('update:numberItem', parseInt(numberItemChild));">
        <template v-slot:prepend>
          <q-icon name="remove" @click="minus()"  v-if="numberItemChild > 0"/>
          <q-icon name="remove" disable v-if="numberItemChild <= 0"/>
        </template>
        <template v-slot:append>
          <q-icon name="add" @click="add()"/>
        </template>
      </q-input>
    </q-item-section>
  </q-item>

</template>

<script>
import { listManagerMixin } from '../mixins/listManagerMixin'

export default {
  name: 'ShoppingItem',
  props: ['numberItem', 'nameItem'],
  data () {
    return {
      val: false,
      numberItemChild: this.numberItem
    }
  },
  created () {
  },
  methods: {
    minus () {
      this.numberItemChild -= 1
      this.$emit('update:numberItem', parseInt(this.numberItemChild))
    },
    add () {
      this.numberItemChild += 1
      this.$emit('update:numberItem', parseInt(this.numberItemChild))
    }
  },
  computed: {
    ingredientList () {
      return this.$store.getters.getIngredientList
    }
  },
  mixins: [listManagerMixin],
  watch: {
    val () {
      if (this.val === true) {
        var ing
        for (const elem in this.ingredientList) {
          if (elem.toLowerCase() === this.nameItem.toLowerCase()) {
            ing = this.ingredientList[elem]
            break
          }
        }
        this.addIngredientToStockList(ing, this.numberItemChild)
      }
    }
  }
}
</script>

<style>
.input_style{
  text-align-last: center;
  max-width: 150px;
}
</style>
