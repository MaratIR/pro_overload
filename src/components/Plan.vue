<template>
  <div class="hello">
    <h2>{{ msg }}</h2>
    <el-card class="box-card">
    <h4>Вес и кол-во подходов за прошлую тренировку</h4>
      <el-form :inline="true" :model="gym" class="demo-form-inline el-icon-edit"> 
        <el-form-item>
          <el-input v-model="gym.last_weight" placeholder="Вес" ></el-input>
        </el-form-item>
        <el-input-number v-model="gym.last_count" @change="handleChange_last" :min="3" :max="6"></el-input-number>
      </el-form>
    <h4>Вес и кол-во подходов на текущей тренировке:</h4>
      <el-form :inline="true" :model="gym" class="demo-form-inline el-icon-edit">
        <el-form-item>
          <el-input v-model="gym.now_weight" placeholder="Вес"></el-input>
        </el-form-item>
        <el-input-number v-model="gym.now_count" @change="handleChange_now" :min="3" :max="6"></el-input-number> 
      </el-form>
      </el-card>
      <el-card>
      <el-form>
        <h4>Тоннаж и кол-во подходов на прошлой и текущей тренировке:</h4>
        <p style="text-align: center">{{gym.last_weight_sum}} кг VS. {{gym.now_weight_sum}} кг</p>
        <el-form-item>
          <el-input :disabled="gym.last_count<1" placeholder="Прошлый подход" v-model="gym.last_reps[0]" class="demo-input-label" @input="summ"></el-input>
          <el-input :disabled="gym.now_count<1" placeholder="Текущий подход" v-model="gym.now_reps[0]" class="demo-input-label" @input="update(0)">
            </el-input>
        </el-form-item>
        <el-form-item>
          <el-input :disabled="gym.last_count<2" placeholder="Прошлый подход" v-model="gym.last_reps[1]" class="demo-input-label" @input="summ"></el-input>
          <el-input :disabled="gym.now_count<2" placeholder="Текущий подход" v-model="gym.now_reps[1]" class="demo-input-label" @input="update(1)"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input :disabled="gym.last_count<3" placeholder="Прошлый подход" v-model="gym.last_reps[2]" class="demo-input-label" @input="summ"></el-input>
          <el-input :disabled="gym.now_count<3" placeholder="Текущий подход" v-model="gym.now_reps[2]" class="demo-input-label" @input="update(2)"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input :disabled="gym.last_count<4" placeholder="Прошлый подход" v-model="gym.last_reps[3]" class="demo-input-label" @input="summ"></el-input>
          <el-input :disabled="gym.now_count<4" placeholder="Текущий подход" v-model="gym.now_reps[3]" class="demo-input-label" @input="(update(3))"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input :disabled="gym.last_count<5" placeholder="Прошлый подход" v-model="gym.last_reps[4]" class="demo-input-label" @input="summ"></el-input>
          <el-input :disabled="gym.now_count<5" placeholder="Текущий подход" v-model="gym.now_reps[4]" class="demo-input-label" @input="update(4)"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input :disabled="gym.last_count<6" placeholder="Прошлый подход" v-model="gym.last_reps[5]" class="demo-input-label" @input="summ"></el-input>
          <el-input :disabled="gym.now_count<6" placeholder="Текущий подход" v-model="gym.now_reps[5]" class="demo-input-label" @input="update(5)"></el-input>
        </el-form-item>
      </el-form>  
      </el-card>
  </div>
</template>


<script>

  export default {

    data() {
      return {
        msg:'Калькулятор прогрессивной сверх нагрузки',
        gym: {
          last_weight: '', //посл вес снаряда - вводим вручну
          last_count: '', //посл кол-во подходов - вводим вручную
          now_weight: '', //текущий вес снаряда - вводим вручную
          now_count: '', //текущее кол-во подходов - вводим вручную
          last_reps_sum: 0, //посл общ кол-во повторений
          now_reps_sum: 0, //тек общ кол-во повторений
          last_weight_sum: 0, //посл общий вес
          now_weight_sum: 0, //текущий общий вес
          last_reps:[], //посл повторения в подходах
          now_reps:[], //тек повторения в подходах
        },
        delta: 0,// остаток повторений от среднего
        repsAverage:0,//средее кол-во повторений
        repSumNowDid:0,//повторений сделано
        repSumNowDo:0,//повторений сделать
        repsAverageDo:0,//среднее кол-во повторений сделать
        deltaDo:0,
      }
    },
    methods: {
      summ() {
        this.gym.last_reps_sum=this.gym.last_reps.reduce((accumulator, value) => accumulator*1 + value*1); //посл общ кол-во повторений
        this.gym.last_weight_sum=this.gym.last_reps_sum*this.gym.last_weight; //посл общий вес
        this.gym.now_reps_sum=Math.ceil(this.gym.last_weight_sum/this.gym.now_weight);//тек общ кол-во повторений
        this.repsAverage=Math.floor(this.gym.now_reps_sum/this.gym.now_count);//средее кол-во подходов
        this.delta=this.gym.now_reps_sum-this.repsAverage*this.gym.now_count;// остаток подходов от среднего
        for (var i=0;i<this.gym.now_count;i++) {
          //кол-во в текущем подходе
          if(this.delta>i){
            this.gym.now_reps[i]=this.repsAverage+1;
          }else{
            this.gym.now_reps[i]=this.repsAverage;
          }
        }
        this.gym.now_reps_sum=this.gym.now_reps.reduce((accumulator, value) => accumulator*1 + value*1);
        this.gym.now_weight_sum=this.gym.now_reps_sum*this.gym.now_weight;
        

      },
      update(value) {
        this.repSumNowDid=0;
        this.repSumNowDo=0;      
        for(let i=0;i<=value;i++){
          this.repSumNowDid=this.repSumNowDid*1+this.gym.now_reps[i]*1;
        }
        console.log('sum:', this.gym.now_reps_sum);
        console.log('did: ',this.repSumNowDid);
        if((this.gym.now_reps_sum-this.repSumNowDid)>0){
          this.repSumNowDo=this.gym.now_reps_sum-this.repSumNowDid;
          this.repsAverageDo=Math.floor(this.repSumNowDo/(this.gym.now_count-(value+1)));
          this.deltaDo=this.repSumNowDo-this.repsAverageDo*(this.gym.now_count-(value+1));
        }else {
          this.repSumNowDo=0;
          this.repsAverageDo=0;
          this.deltaDo=0;
        }
        // if (Math.floor(this.repSumNowDo/(this.gym.now_count-(value+1)))){
        //   this.repsAverageDo=Math.floor(this.repSumNowDo/(this.gym.now_count-(value+1)));
        //   this.deltaDo=this.repSumNowDo-this.repsAverageDo*(this.gym.now_count-(value+1));
        // }else {
        //   this.repsAverageDo=0;
        //   this.deltaDo=0;
        // };
        console.log('aver: ',this.repsAverageDo);
        //this.deltaDo=this.repSumNowDo-this.repsAverageDo*(this.gym.now_count-(value+1));
        console.log('count: ', this.gym.now_count);
        console.log('do: ',this.repSumNowDo);
        console.log('delta: ',this.deltaDo);
        for (let i=value+1;i<this.gym.now_count;i++) {
            this.gym.now_reps[i]=this.repsAverageDo;
        }
        for(let i=value+1;i<this.gym.now_count;i++) {
          if(this.deltaDo){
            this.gym.now_reps[i]=this.gym.now_reps[i]+1;
            this.deltaDo=this.deltaDo-1;
          }
        }
        this.gym.now_weight_sum=this.gym.now_reps.reduce((accumulator, value) => accumulator*1 + value*1)*this.gym.now_weight;
      },
      handleChange_last() {
        // for(var i = 1; i <= this.num_last;i++){
        //   this.last.push('');
        //}
      },
      handleChange_now() {
        // for(var i = 1; i <= this.num_last;i++){
        //   this.now.push('');
        //}
      }

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
  .demo-input-label {
    display: inline-block;
    width: 130px;
  }

</style>
