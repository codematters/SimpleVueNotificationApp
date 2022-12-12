<template>
  <button class="btn" @click="showCoffee = true">Want Coffee?</button>
  <Coffee 
    message= "Message has been sent" 
    :show="showCoffee" 
    @hide="showCoffee = false" 
    type="One Cup of Delicious Coffee is Brewed!"
    position="top-right"
  />
</template>


<script>
import Coffee from "./components/coffee/Coffee.vue";
export default {
  components: {Coffee},
  data: () => ({
    showCoffee: false
  })

};

</script>


<style>


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  
}

.btn {
  background:#cec8ef;
  padding: 15px;
  border-radius: 25px;
  border: none;
}

.btn:hover{
  background:#8878c3;
  opacity: 80%;
}

</style>