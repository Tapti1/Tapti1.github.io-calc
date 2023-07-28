<template>
    <div class="content-center rounded-lg"
    :class="get_bg">    
        <div class="ml-4 font-bold" :class="get_calc_text">
            calc
            
            <label class="ml-3 mt-5 relative inline-flex items-center cursor-pointer">
            <input type="checkbox" v-model="theme" class="sr-only peer">
            <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-gray-300 dark:peer-focus:ring-indigo-800 rounded-full peer dark:bg-indigo-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-indigo-500"></div>
            </label>

        </div>
        <div>
            <div class="text-right rounded-lg m-3 p-2 w-1/2"
            :class="get_futer">
                {{calc_str}}
            </div>
            <div class="text-center rounded-lg m-3 p-2 w-1/2"
            :class="get_main">

                <button @click="addNumder(7)" :class="get_num_button" class="rounded-lg m-2 p-2 w-1/5 font-bold ">7</button>
                <button @click="addNumder(8)" :class="get_num_button" class="rounded-lg m-2 p-2 w-1/5 font-bold ">8</button>
                <button @click="addNumder(9)" :class="get_num_button" class="rounded-lg m-2 p-2 w-1/5 font-bold ">9</button>
                <button @click="delNumber()" :class="get_del_button" class="rounded-lg m-2 p-2 w-1/5">DEL</button>
                <br>

                <button @click="addNumder(4)" :class="get_num_button" class="rounded-lg m-2 p-2 w-1/5 font-bold ">4</button>
                <button @click="addNumder(5)" :class="get_num_button" class=" rounded-lg m-2 p-2 w-1/5 font-bold ">5</button>
                <button @click="addNumder(6)" :class="get_num_button" class=" rounded-lg m-2 p-2 w-1/5 font-bold ">6</button>
                <button @click="setOperation('+')" :class="get_num_button" class=" rounded-lg m-2 p-2 w-1/5 font-bold ">+</button>
                <br>

                <button @click="addNumder(1)" :class="get_num_button" class="rounded-lg m-2 p-2 w-1/5 font-bold ">1</button>
                <button @click="addNumder(2)" :class="get_num_button" class=" rounded-lg m-2 p-2 w-1/5 font-bold ">2</button>
                <button @click="addNumder(3)" :class="get_num_button" class=" rounded-lg m-2 p-2 w-1/5 font-bold ">3</button>
                <button @click="setOperation('-')" :class="get_num_button" class=" rounded-lg m-2 p-2 w-1/5 font-bold ">-</button>
                <br>

                <button @click="setDot()" :class="get_num_button" class=" rounded-lg m-2 p-2 w-1/5 font-bold ">.</button>
                <button @click="addNumder(0)" :class="get_num_button" class="rounded-lg m-2 p-2 w-1/5 font-bold ">0</button>
                <button @click="setOperation('/')" :class="get_num_button" class="rounded-lg m-2 p-2 w-1/5 font-bold ">/</button>
                <button @click="setOperation('x')" :class="get_num_button" class="rounded-lg m-2 p-2 w-1/5 font-bold ">x</button>
                <br>
                
                <button @click="reset()" :class="get_del_button" class="rounded-lg m-2 p-2 w-1/3">RESET</button>
                <button @click="calculate()" :class="get_reset_button" class="rounded-lg m-2 p-2 w-1/3 font-bold ">=</button>
                
            </div>
        </div>
    </div>
</template>

<script>
export default{
    name:"Calc",
    data(){
        return{
            number:0,
            calc_str:"",   
            is_point:false,  
            is_operation:false, 
            type_of_operation:"",   
            theme:false,
        }
    },
    methods:{
        addNumder(dig){
            this.calc_str+=dig
        },
        delNumber(){
            if(this.calc_str[this.calc_str.length-1]=='.')  this.is_point=false
            if(this.calc_str[this.calc_str.length-1]=='x' || this.calc_str[this.calc_str.length-1]=='/' || this.calc_str[this.calc_str.length-1]=='+' || this.calc_str[this.calc_str.length-1]=='-')  this.is_operation=false
            this.calc_str=this.calc_str.slice(0,-1)
        },
        setDot(){
            if(this.calc_str.length==0 || this.calc_str[this.calc_str.length-1]=='x' || this.calc_str[this.calc_str.length-1]=='/' || this.calc_str[this.calc_str.length-1]=='+' || this.calc_str[this.calc_str.length-1]=='-')
                this.calc_str+="0"
            if(!this.is_point) this.calc_str+="."
            this.is_point=true
        },
        setOperation(type){
            if(!this.is_operation){                
                this.is_point=false
                this.is_operation=true
                this.calc_str+=type
            }
            else if(this.calc_str[this.calc_str.length-1]=='x' || this.calc_str[this.calc_str.length-1]=='/' || this.calc_str[this.calc_str.length-1]=='+' || this.calc_str[this.calc_str.length-1]=='-') {
                console.log(this.calc_str.length-1)
                this.calc_str=this.calc_str.slice(0,-1)+type
            }
            else{
                this.calculate()
                this.is_point=false
                this.is_operation=true
                this.calc_str+=type
            }
            this.type_of_operation=type
        },
        reset(){

            this.calc_str=""
            this.is_point=false
            this.is_operation=false
        },
        calculate(){
            if(this.is_operation){
                let ans=0
                let numbers=this.calc_str.split(this.type_of_operation)

                switch(this.type_of_operation){
                    case '+':
                        ans=Number(numbers[0])+Number(numbers[1])
                        break
                    case '-':
                        ans=Number(numbers[0])-Number(numbers[1])
                        break
                    case 'x':
                        ans=Number(numbers[0])*Number(numbers[1])
                        break
                    case '/':
                        ans=Number(numbers[0])/Number(numbers[1])
                        break
                    
                }

                this.calc_str=ans.toString()
                this.is_operation=false
                this.is_point=this.calc_str.includes(".") ? true : false
            }
        },
    },
    computed:{
        get_bg(){
            switch(this.theme){
                case false:
                    return "bg-blue-400"
                case true:
                    return "bg-gray-100"
                case "3":
                    return ""
            }
        },
        get_futer(){
            switch(this.theme){
                case false:
                    return "bg-blue-900 text-white"
                case true:
                    return "bg-white text-yellow-900"
                case "3":
                    return ""
            }
        },
        get_main(){
            switch(this.theme){
                case false:
                    return "bg-blue-900 text-white"
                case true:
                    return "bg-gray-300 text-yellow-900"
                case "3":
                    return ""
            }
        },
        get_num_button(){
            switch(this.theme){
                case false:
                    return "bg-yellow-100 text-blue-900 hover:bg-white"
                case true:
                    return "bg-gray-200 text-yellow-900 hover:bg-white"
                case "3":
                    return ""
            }
        },
        get_del_button(){
            switch(this.theme){
                case false:
                    return "bg-blue-400 text-white hover:bg-blue-500"
                case true:
                    return "bg-indigo-400 text-white hover:bg-indigo-500"
                case "3":
                    return ""
            }
        },
        get_reset_button(){
            switch(this.theme){
                case false:
                    return "bg-red-400 text-white hover:bg-red-500"
                case true:
                    return "bg-yellow-500 text-white hover:bg-yellow-600"
                case "3":
                    return ""
            }
        },
        get_calc_text(){
            switch(this.theme){
                case false:
                    return "text-blue-900"
                case true:
                    return "text-yellow-900"
                case "3":
                    return ""
            }
        }
    },
}
</script>