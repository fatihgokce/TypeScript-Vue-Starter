<!-- src/components/HelloDecorator.vue -->
<!-- This is an alternative way to define the Hello component using decorators -->
<template>
    <div>
        <div class="greeting">Hello23 {{name}}{{exclamationMarks}}</div>
        <button @click="decrement">-</button>
        <button @click="increment">+</button>
        <button @click="openPicker">open</button>
      <datepicker ref="dp" :value="state.date" name="uniquename"></datepicker>
    </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from "vue-property-decorator";
import Datepicker  from "vuejs-datepicker";  


@Component({
  components: {
  Datepicker
  },
})
export default class HelloDecorator extends Vue {
    @Prop() name!: string;
    @Prop() initialEnthusiasm!: number;

    enthusiasm = this.initialEnthusiasm;
    state = {date: new Date(2017, 9,  16)};
      $refs!: {
        checkboxElement: HTMLFormElement,
        dp:any
    }
    async created() {
        console.log("created");  
        this.fetchData();      
    }
    async fetchData(){
        debugger;
       let data= await fetch('https://jsonplaceholder.typicode.com/users');
       let js=await data.json();
       console.log(js);
    }
    increment() {
        this.enthusiasm++;
    }
    decrement() {
        if (this.enthusiasm > 1) {
            this.enthusiasm--;
        }
    }
    openPicker(){
        this.$refs.dp.showCalendar();
    }
    get exclamationMarks(): string {
        return Array(this.enthusiasm + 1).join('!');
    }
}
</script>

<style>
.greeting {
    font-size: 20px;
}
</style>
