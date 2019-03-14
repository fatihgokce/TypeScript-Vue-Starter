<!-- src/components/HelloDecorator.vue -->
<!-- This is an alternative way to define the Hello component using decorators -->
<template>
  <div>
    <div class="greeting">Hello23 {{name}}{{exclamationMarks}}</div>
    <button @click="decrement">-</button>
    <button @click="increment">+</button>
    <button @click="openPicker">open</button>
    <datepicker ref="dp" :value="state.date" name="uniquename"></datepicker>
  
     <label class="label" for="email">Email</label>
        <p :class="{ 'control': true }">
            <input v-validate="'required|email'" :class="{'input': true, 'is-danger': errors.has('email') }" name="email" type="text" placeholder="Email">
            <span v-show="errors.has('email')" class="help is-danger">{{ errors.first('email') }}</span>
        </p>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch,Inject } from "vue-property-decorator";
import Datepicker from "vuejs-datepicker";
//import { Validator ,VerifyOptions,directive} from "vee-validate";
import VeeValidate,{Validator} from "vee-validate";

Vue.use(VeeValidate, { inject: false });
//const v = new Validator();

@Component({
  components: {
    Datepicker,
    
  },
   $_veeValidate: { validator: "new" }
})
export default class HelloDecorator extends Vue {
  @Prop() name!: string;
  @Prop() initialEnthusiasm!: number;
  //errors: string[] = [];
  enthusiasm = this.initialEnthusiasm;
  state = { date: new Date(2017, 9, 16) };
//   name2: string = "email";
//   rules: string = "email";
//   email: string = "";
//  errors2: string[] = [];
//   @Watch("email")
//   async onEmailChanged(val) {
   
//     if (val) {
//       await this.verify(val);
//     }
//   }

//   async verify(value) {
      
//     const { errors } = await v.verify(value, this.rules,<VerifyOptions>{name:this.name2});
//     this.errors2 = <string[]>errors;
//   }
@Inject('$validator') public $validator!: Validator;
  $refs!: {
    checkboxElement: HTMLFormElement;
    dp: any;
  };
  async created() {
    console.log("created");
    this.fetchData();
  }
  async fetchData() {
    let endPoint = await fetch(
      "http://localhost:24514/tr/tourvisio/apiendpoint",
      { mode: "no-cors" }
    );

    //console.log(endPoint.json());
    let data = await (await fetch(
      "https://jsonplaceholder.typicode.com/users"
    )).json();
    //    let js=await data.json();
    console.log(data);
  }
  increment() {
    this.enthusiasm++;
  }
  decrement() {
    if (this.enthusiasm > 1) {
      this.enthusiasm--;
    }
  }
  openPicker() {
    this.$refs.dp.showCalendar();
  }
  get exclamationMarks(): string {
    return Array(this.enthusiasm + 1).join("!");
  }
}
</script>

<style>
.greeting {
  font-size: 20px;
}
</style>
