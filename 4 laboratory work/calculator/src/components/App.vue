<template>
    <Page  class="app">
     <ActionBar title="Calculator"/>
         <GridLayout class="keyboard" columns="*, *, *, *" rows="*, *, *, *, *, *">
            <Label textWrap="true" class="result" v-model="result" row="0" col="0" colSpan="4"/>/>
            <Button  class="btn act" text="C" @tap="del()" row="1" col="0" colSpan="3"/>
            <Button  class="btn act" text="/" @tap="input('/')" row="1" col="3"/>
            <Button  class="btn" text="7" @tap="input('7')" row="2" col="0"/>
            <Button  class="btn" text="8" @tap="input('8')" row="2" col="1"/>
            <Button  class="btn" text="9" @tap="input('9')" row="2" col="2"/>
            <Button  class="btn act" text="*" @tap="input('*')" row="2" col="3"/>
            <Button  class="btn" text="4" @tap="input('4')" row="3" col="0"/>
            <Button  class="btn" text="5" @tap="input('5')" row="3" col="1"/>
            <Button  class="btn" text="6" @tap="input('6')" row="3" col="2"/>
            <Button  class="btn act" text="-" @tap="input('-')" row="3" col="3"/>
            <Button  class="btn" text="1" @tap="input('1')" row="4" col="0"/>
            <Button  class="btn" text="2" @tap="input('2')" row="4" col="1"/>
            <Button  class="btn" text="3" @tap="input('3')" row="4" col="2"/>
            <Button  class="btn act" text="+" @tap="input('+')" row="4" col="3"/>
            <Button  class="btn" text="0" @tap="input('0')" row="5" col="0" colSpan="2"/>
            <Button  class="btn" text="." @tap="input('.')" row="5" col="2"/>
            <Button  class="btn act" text="=" @tap="calc()" row="5" col="3"/>
         </GridLayout>
    </Page>
</template>

<script >
  let operation = false;
  let point = false;
  let zero = true
   export default  {
    data() {
        return {
            result: '',
      }
    },
    methods:{
            input: function(char){
                this.result = this.result.toString();
                 if (this.result === 'invalid input'){
                    this.result = '';
                }
                if ((char === '+' || char === '/' || char === '-' || char === '*') && operation === false){
                    operation = true;
                    point = false;
                    zero = true;
                    this.result += char;
                }
                else if(char === '.' && point === false){
                    if (zero === true){
                        this.result += 0;
                        zero = false;
                    }
                    operation = true;
                    point = true;
                    this.result += char;
                }
                else if ((char === '0' || char === '1' || char === '2' || char === '3' || char === '4' || 
                          char === '5' || char === '6' || char === '7' || char === '8' || char === '9')){
                    operation = false;  
                    zero = false;                 
                    this.result += char;
                }
            },
            calc: function(){
                try{
                    this.result = eval(this.result);
                    if (isNaN(this.result)) {
                        throw new Error();
                  }
                }
                catch{
                    this.result = 'invalid input';
                }

            },
            del: function(){
                zero = true;
                point = false;
                operation = true;
                this.result = '';
            }
    }
  }
</script>

<style scoped>
  ActionBar{
    background-color: #252525;
    color: #fff
  }
    .app{
    background-color: #252525;
    }
    .result{
        text-align: right;
        font-size: 28px;
        overflow-wrap: break-word;
        color: #fff;
    }
    .btn{
        background-color: #243849;
        color: #FFF;
        border: solid;
        border-color: #182530;
        border-width: 2px;
    }
    .btn:active {
         background-color: #182530;
    }
    .act {
        background-color: rgb(11, 117, 211);
        color: #FFF;
    }
     .act:active {
            background-color: #182530;
        }
</style>