<template actionBarHidden="true">
    <Page actionBarHidden="true"  class="app">
      <StackLayout>
        <Label class='city'  :text='listOfItems[this.selectedItem]' @tap='city();'/>
        <ScrollView orientation="vertical"> 
           <StackLayout class='forecast' orientation="vertical">
              <Image class='img' :src="imagePath" stretch="none"/>
              <label class='text'> Температура: {{weather.fact.temp}}</label>
              <label class='text'> ощущается: {{weather.fact.feels_like}}</label>
              <label class='text'> Ветер {{weather.fact.wind_speed }} m/c </label>
              <label class='text'> Влажность {{weather.fact.humidity}}%</label>
              <label class='text'> Давление {{weather.fact.pressure_mm}} мм рт. ст.</label>
           </StackLayout>
        </ScrollView>
      </StackLayout>
    </Page>
</template>

<script >
import { Http } from '@nativescript/core'
import * as ApplicationSettings from "application-settings";
  export default {
    data() {
      return {
        listOfItems: [
          { title: "Ханты-Мансийск",
            toString: () => {
              return 'Ханты-Мансийск';
            },
            latitude: 61.0041700,
            longitude: 69.0019400
          }, 
          { title: "Сургут",
            toString: () => {
              return 'Сургут';
            },
            latitude: 61.2579196,
            longitude: 73.3987199
          },
          { title: "Мегион",
            toString: () => {
              return 'Мегион';
            },
            latitude: 61.0332515,
            longitude: 76.0659573
          },
          { title: "Нижневартовск",
            toString: () => {
              return 'Нижневартовск';
            },
            latitude: 60.9264987,
            longitude: 76.5088749
          },
          { title: "Урай",
            toString: () => {
              return 'Урай';
            },
            latitude: 60.1249573,
            longitude: 64.7970044
          },
          { title: "Лянтор",
            toString: () => {
              return 'Лянтор';
            },
            latitude: 61.6200639,
            longitude: 72.142693
          },
          { title: "Нефтеюганск",
            toString: () => {
              return 'Нефтеюганск';
            },
            latitude: 61.0977284,
            longitude: 72.5050062
          },
          { title: "Югорск",
            toString: () => {
              return 'Югорск';
            },
            latitude: 61.3174074,
            longitude: 63.3027627
          },
          { title: "Советский",
            toString: () => {
              return 'Советский';
            },
            latitude: 61.3730853,
            longitude: 63.5316841
          },
          { title: "Нягань",
            toString: () => {
              return 'Нягань';
            },
            latitude: 62.1398738,
            longitude: 65.3460493
          },
        ],
        selectedItem: 0,
        weather: {
          fact: {
            temp : 0,
            feels_like : 0,
            icon : 'name',
            condition : 'condition',
            wind_speed : 0,
            wind_dir : 'wind_dir',
            pressure_mm	: 0,
            }
        },
        imagePath: '',
        cities:['Ханты-Мансийск', 'Сургут', 'Мегион', 'Нижневартовск', 'Урай', 'Лянтор', 'Нефтеюганск', 'Югорск', 'Советский', 'Нягань']
      }
    },
    mounted(){
      if(ApplicationSettings.getString('weather')){
        this.weather.fact=JSON.parse(ApplicationSettings.getString('weather'));
        console.log('Погода загружена');
      }
      if(ApplicationSettings.getString('city')){
        this.selectedItem=JSON.parse(ApplicationSettings.getString('city'));
        console.log('Место загружено');
      }
      else{
        this.city()
      }
      this.imagePath = '~/res/' + String(this.weather.fact.condition) + '.png';
    },
    methods:{
      city(){
        action("Выберите город:", "отмена", this.cities)
        .then(result => {
          this.selectedItem = this.cities.indexOf(result);
          console.log(this.selectedItem )
          Http.request({
            url: 'https://api.weather.yandex.ru/v2/forecast?limit=1&lat=' +  String(this.listOfItems[this.selectedItem].latitude) + '&lon=' + String(this.listOfItems[this.selectedItem].longitude),
            method: "GET",
            headers: {"X-Yandex-API-Key": "f14cccc0-8292-432d-b783-7b31912dc7ea"},
          }).then(
            (response) => {
              this.weather = response.content.toJSON();
            this.imagePath = '~/res/' + String(this.weather.fact.condition) + '.png';
            console.log(`Путь изображения: ${this.imagePath}`)
          });
          ApplicationSettings.setString('city', JSON.stringify(this.selectedItem));        
        });
      }
    }
  }
</script>

<style scoped>
.app{
    background-color: #212529;;
}
.forecast{
  font-size: 20;
  margin: 50px 70px;
  padding: 20px;
  padding-bottom: 90px;
  background-color: #243849;
  border: solid;
  border-color: #182530;
  border-width: 4px;
}
.img{
  margin: 50px 32%;
}
.text{
    margin: 50px auto;
    background-color: rgb(29, 100, 182);
    border: solid;
    border-color: #182530;
    border-width: 4px;
    padding: 40px;
}
.city{
  font-size: 25;
  text-align: center;
  height: 200px;
  margin: 50px 70px;
  padding: 50px;
  border: solid;
  border-color: #182530;
  border-width: 4px;
  background-color: rgb(29, 100, 182);
}
</style>