<script setup lang="ts">
import { useQueryClient, useQuery, useMutation } from '@tanstack/vue-query';
import { ref } from 'vue';

//data
const param = ref('');

//composables
const { isPending, isError, data, error } = useQuery({
  queryKey: ['todos', param],
  queryFn: () => getTodos(param),
});

const getTodos = async (param: any) => {
  const res = await fetch(`https://62e737420d7b3479.mokky.dev/all${param.value}`);
  const data = await res.json();
  return data;
};

//functions
const switchTag = (tag: any) => {
  //проверка на существование
  if (param.value.includes(tag)) {
    //если есть ?
    if (param.value[param.value.indexOf(tag[0]) - 1] === '?') {
      tag = '?' + tag;
      param.value = param.value.replace(tag, '');
      // если удаляем последний ?
      if (param.value.includes('&') && !param.value.includes('?')) {
        console.log('kek');
        param.value = param.value.replace('&', '?');
      }
      console.log('1');
      return;
    }
    //иначе &
    tag = '&' + tag;
    param.value = param.value.replace(tag, '');
    console.log('2');
    return;
  }
  //проверка на первость
  if (param.value === '') {
    param.value = '?' + tag;
    console.log('3');
    return;
  }
  // иначе просто добавляем
  param.value = param.value + '&' + tag;
  console.log('4');
};

const clickTag = (text: string) => {
  if (text === 'Frontend') {
    switchTag('role=*Frontend');
  }
  if (text === 'Fullstack') {
    switchTag('role=*Fullstack');
  }
  if (text === 'CSS') {
    switchTag('languages=*CSS');
  }
  if (text === 'JavaScript') {
    switchTag('languages=*JavaScript');
  }
  if (text === 'HTML') {
    switchTag('languages=*HTML');
  }
  if (text === 'Junior') {
    switchTag('level=*Junior');
  }
  if (text === 'Midweight') {
    switchTag('level=*Midweight');
  }
  if (text === 'Senior') {
    switchTag('level=*Senior');
  }
};

const clearFn = ()=>{
  param.value = ''
}
</script>

<template>
  <img class="imageBg" src="./assets/bg-header-desktop.svg" />
  <div class="Main">
    <div class="container">
      <div class="btns">
        <div class="rightBtn">
        <button
          class="btn"
          :class="{ ss: param.includes('role=*Frontend') }"
          @click="switchTag('role=*Frontend')">
          Frontend
        </button>
        <button
          class="btn"
          :class="{ ss: param.includes('role=*Fullstack') }"
          @click="switchTag('role=*Fullstack')">
          Fullstack
        </button>
        <button
          class="btn"
          :class="{ ss: param.includes('languages=*CSS') }"
          @click="switchTag('languages=*CSS')">
          CSS
        </button>
        <button
          class="btn"
          :class="{ ss: param.includes('languages=*JavaScript') }"
          @click="switchTag('languages=*JavaScript')">
          JavaScript
        </button>
        <button
          class="btn"
          :class="{ ss: param.includes('languages=*HTML') }"
          @click="switchTag('languages=*HTML')">
          HTML
        </button>
        <button
          class="btn"
          :class="{ ss: param.includes('level=*Junior') }"
          @click="switchTag('level=*Junior')">
          Junior
        </button>
        <button
          class="btn"
          :class="{ ss: param.includes('level=*Midweight') }"
          @click="switchTag('level=*Midweight')">
          Midweight
        </button>
        <button
          class="btn"
          :class="{ ss: param.includes('level=*Senior') }"
          @click="switchTag('level=*Senior')">
          Senior
        </button>
      </div>
      <div class="clear">
        <button @click="clearFn" class="btn">Clear</button>
      </div>
      </div>
      <div class="rowss" v-if="!isPending && !isError">
        <div class="sh" v-for="item of data">
          <div class="jc">
            <div>
              <img :src="item.logo" />
            </div>
            <div class="flexMain">
              <div class="flex">
                <div class="wh m">
                  <span class="nameComp">{{ item.company }}</span>
                  <span class="new">{{ item.new ? 'NEW!' : '' }}</span>
                  <div class="featured">{{ item.featured ? 'FEATURED' : '' }}</div>
                </div>

                <div class="wh z">
                  {{ item.position }}
                </div>

                <div class="wh op">
                  {{ item.postedAt + ' •' }}
                  {{ item.contract + ' •'}}
                  {{ item.location }}
                </div>
              </div>
            </div>
            <div class="flexEnd">
              <div :class="{ ss: param.includes(item.role) }" class="btn" @click="clickTag(item.role)">{{ item.role }}</div>
              <div v-for="it in item.languages">
                <div :class="{ ss: param.includes(it) }" class="btn" @click="clickTag(it)">{{ it }}</div>
              </div>
              <div :class="{ ss: param.includes(item.level) }" class="btn" @click="clickTag(item.level)">{{ item.level }}</div>
            </div>
          </div>
        </div>
      </div>
      <div v-else class="loader">
        <div class="lds-ripple"><div></div><div></div></div>
      </div>
      <div v-if="data.length === 0">
        <h1>No data</h1>
      </div>
    </div>
  </div>
</template>

<style scoped>
.Main {
  display: flex;
  justify-content: center;
  background-color: #f0fafb;
}
.container {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  width: 90%;
}
.rightBtn{
  
}
.clear{

}
.m {
  display: flex;
  gap: 10px;
}
.sh{
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  border-radius: 3cqh;
}
.btn {
  background-color: #f0fafb;
  border: none;
  border-radius: 8%;
  padding: 10px;
  margin: 10px;
  cursor: pointer;
  font-weight: bold;
  color: #61a59f;
}
.nameComp {
  color: #61a59f;
}
.z {
  color: black;
  font-size: 20px;
  font-weight: bold;
}
.btns {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  min-width: 100%;
  background-color: white;
  height: 60px;
  margin-top:-35px;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  border-radius: 10px;
  justify-content: space-between;
}
.btn:hover{
  background-color: #60a2a7;
  color: white;
}
.op{
  opacity: 0.5;
}
.new {
  color: white;
  background-color: #5da5a4;
  padding-left: 10px;
  padding-right: 10px;
  padding-top: 5px;
  padding-bottom: 5px;
  border-radius: 25%;
  font-size: 11px;
  align-self: baseline;
}
.rowss {
  min-width: 100%;
}
.featured {
  color: white;
  background-color: black;
  padding-left: 10px;
  padding-right: 10px;
  padding-top: 5px;
  padding-bottom: 5px;
  border-radius: 25%;
  font-size: 11px;
  align-self: baseline;
}
.ss {
  background-color: #60a2a7;
  color: white;
}
.jc {
  display: grid;
  grid-template-columns: 0.5fr 2fr 2fr;
  background-color: white;
  margin-bottom: 20px;
  margin-top: 20px;
  border-radius: 10px;
  padding: 20px;
}
.flexMain {
  display: flex;
  align-items: stretch;
}
.flex {
  display: flex;
  flex-wrap: wrap;
}
.flexEnd {
  display: flex;
  justify-content: end;
  align-items: center;
  gap: 15px;
}
.wh {
  width: 100%;
}
.imageBg {
  background-color: #5da5a4;
  width: 100%;
}



.lds-ripple {
  margin-top: 50px;
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-ripple div {
  position: absolute;
  border: 4px solid #fff;
  opacity: 1;
  border-radius: 50%;
  animation: lds-ripple 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;
}
.lds-ripple div:nth-child(2) {
  animation-delay: -0.5s;
}
@keyframes lds-ripple {
  0% {
    top: 36px;
    left: 36px;
    width: 0;
    height: 0;
    opacity: 0;
  }
  4.9% {
    top: 36px;
    left: 36px;
    width: 0;
    height: 0;
    opacity: 0;
  }
  5% {
    top: 36px;
    left: 36px;
    width: 0;
    height: 0;
    opacity: 1;
  }
  100% {
    top: 0px;
    left: 0px;
    width: 72px;
    height: 72px;
    opacity: 0;
  }
}

</style>
