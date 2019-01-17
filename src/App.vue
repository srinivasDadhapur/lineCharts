<template>
  <div>

<div class="welcome">
<h1>Draw Line Charts Here Using CSV</h1>
<p>select a file from the computer and click on upload, You will see the line graphs on the below empty graph</p>
</div>

<div id="chart">
  <line-chart v-bind:data="chartData"></line-chart>
</div>


<dvi id="formDiv">
  <form method="post">
    <label>File
  <input type="file" id="file" v-on:change="handleFileUpload($event)"/>
  </label>
  <button @click.prevent="submitFile">Upload</button>
  </form>
</dvi>

  </div>
</template>

<script>
export default {
data(){
  return {
    chartData :[],
    file:'',
  }
},
methods:{
  handleFileUpload:function(event){
    //take the uploaded filename into file property
    this.chartData = [];
    this.file = event.target.files[0];
  },
  submitFile:function(){
    //read the data and plot the line graph
    let reader = new FileReader()
    reader.readAsText(this.file)
    reader.onload = (event)=>{
      this.file = event.target.result.split('\n');

      for(let i=0;i<this.file.length;i++){
        let linechart = {
           name:'',
           data:[]
         }
        linechart.name = this.file[i].split(',')[0];
        let lines = this.file[i].split(',');

          for(let j=1;j<lines.length;j++){
            if(lines[j].split('|').length==2){
            let key = parseInt(lines[j].split('|')[0]);
            let value = parseInt(lines[j].split('|')[1]);
            linechart.data.push([key,value])
          }else{
            continue
          }
          }
        this.chartData.push(linechart)
      }
    }
    //send the file to the server here
    //using $http request
  }
}

}



</script>

<style scoped>
#formDiv{
  text-align:center;
  margin-top:10px 0;
  padding:10px;

  width:100%;
  height:100%;

}
.welcome{
  text-align:center;
}
#chart{
display:inline-block;
text-align:center;
width:70%;
height:100%;
margin-left:12.5%;
border:2px solid grey;
border-radius: 30px;
box-shadow: 1px 1px 5px gray;
}

</style>
