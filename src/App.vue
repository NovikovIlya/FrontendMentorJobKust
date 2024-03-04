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
console.log(data);
const getTodos = async (param) => {
  const res = await fetch(
    `https://62e737420d7b3479.mokky.dev/all${param.value}`
  );
  const data = await res.json();
  return data;
};

//functions
const switchTag = (tag) => {
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

const clickTag = (text) => {
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
</script>

<template>
  <button
    :class="{ ss: param.includes('role=*Frontend') }"
    @click="switchTag('role=*Frontend')"
  >
    Frontend
  </button>
  <button
    :class="{ ss: param.includes('role=*Fullstack') }"
    @click="switchTag('role=*Fullstack')"
  >
    Fullstack
  </button>
  <button
    :class="{ ss: param.includes('languages=*CSS') }"
    @click="switchTag('languages=*CSS')"
  >
    CSS
  </button>
  <button
    :class="{ ss: param.includes('languages=*JavaScript') }"
    @click="switchTag('languages=*JavaScript')"
  >
    JavaScript
  </button>
  <button
    :class="{ ss: param.includes('languages=*HTML') }"
    @click="switchTag('languages=*HTML')"
  >
    HTML
  </button>
  <button
    :class="{ ss: param.includes('level=*Junior') }"
    @click="switchTag('level=*Junior')"
  >
    Junior
  </button>
  <button
    :class="{ ss: param.includes('level=*Midweight') }"
    @click="switchTag('level=*Midweight')"
  >
    Midweight
  </button>
  <button
    :class="{ ss: param.includes('level=*Senior') }"
    @click="switchTag('level=*Senior')"
  >
    Senior
  </button>
  <div v-if="!isPending && !isError">
    <div v-for="item of data">
      <div class="jc">
        <div>
          <img :src="item.logo" />
        </div>
        <div class="flexMain">
          <div class="flex">
            <div class="wh">
              {{ item.company }}
              {{ item.new ? 'New' : '' }}
              {{ item.featured ? 'Featured' : '' }}
            </div>

            <div class="wh">
              {{ item.position }}
            </div>

            <div class="wh">
              {{ item.postedAt }}
              {{ item.contract }}
              {{ item.location }}
            </div>
          </div>
        </div>
        <div>
          <div @click="clickTag(item.role)">{{ item.role }}</div>
          <div v-for="it in item.languages">
            <div @click="clickTag(it)">{{ it }}</div>
          </div>
          <div @click="clickTag(item.level)">{{ item.level }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.ss {
  background-color: white;
}
.jc {
  display: grid;
  grid-template-columns: 1fr 2fr 2fr;
}
.flexMain {
  display: flex;
  align-items: stretch;
}
.flex {
  display: flex;
  flex-wrap: wrap;
}
.wh {
  width: 100%;
}
</style>
