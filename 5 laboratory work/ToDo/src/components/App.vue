<template>
  <Page  class="app">
    <ActionBar title="TODO!"/>
    <StackLayout>
      <TextField class="input" v-model="newText" hint="Введите задачу"  @returnPress= "newTask()" col="0"/>
      <button class="btn add" text="Добавить" @tap="newTask()" col="1"/>
      <ScrollView orientation="horizontal">
        <ListView  class="task" for="task in tasks">
          <v-template>
            <GridLayout columns="200%, 70, 70">
            <label  class="task-text done" v-if="task.done" textWrap="true" col="0">{{task.title}}</label>
            <label  class="task-text" v-else  @tap="edit(task.id, task.title)" textWrap="true" col="0">{{task.title}}</label>
            <Button  class="btn" text="\/" @tap="taskDone(task.id)" col="1"/>
            <Button  class="btn" text="X" @tap="remove(task.id)" col="2"/> 
            </GridLayout>
          </v-template>
        </ListView>
      </ScrollView>
    </StackLayout>
  </Page>
</template>

<script>
import * as ApplicationSettings from "application-settings";
export default {
  data () {
    return {
      newText: '',
      tasks: []
    }
  },
  mounted(){
    if(ApplicationSettings.getString('tasks')){
      this.tasks=Object.values(JSON.parse(ApplicationSettings.getString('tasks')));
    }
  },
  methods: {
    newTask () {
      if(this.newText != ''){
        this.tasks.push({
          id: Math.random(),
          title: this.newText,
          done: false
        });
        this.newText = '';
      }
      this.save();
    },
    taskDone (id) {
      this.tasks = this.tasks.map(task => {
        if (task.id == id) task.done = !task.done;
        return task;
      })
      this.save();
    },
    remove (id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
      this.save();
    },
    save(){
      let toSave = Object.assign({}, this.tasks);
      ApplicationSettings.setString('tasks', JSON.stringify(toSave));
    },
    
    edit(id, old_text) {
      prompt({
        title: "Изменение задачи",
        message: "Новая задача:",
        okButtonText: "OK",
        cancelButtonText: "Отмена",
        defaultText: old_text,
      })
      .then(result => {
        this.tasks.forEach(task => {
          if (task.id == id && result.text != ''){
            task.title = result.text;
            this.save();
          }    
        });
      })
    }
  }
}
</script>

<style>
ActionBar{
  background-color: #252525;
  color: #fff
}
.app{
  background-color: #252525;
}
.task-text{
  margin: 30px 20px;
  background-color: #243849;
  color: #fff;
  text-align: center;
  border: solid;
  border-color: #182530;
  border-width: 4px;
}
.done {
  text-decoration: line-through;
  background-color: rgb(11, 117, 211);
}
.btn{
  background-color: rgb(11, 117, 211);
  border: solid;
  border-color: #182530;
  border-width: 4px;
  color: #fff;
  margin: 30px 10px;
}
.btn:active {
  background-color: #182530;
}
.add{
  width: 1020px;
}
.input{
  background-color: #243849;
  border: solid;
  border-color: #182530;
  border-width: 4px;
  color: #fff;
  margin: 50px 30px;
}
.input:focus{
  background-color: #243849;
}
</style>
