<template>
  <q-page class="main-container-q-page">
    <ul>
      <li v-for="user in users" :key="user.id">
        <div>
          <img :src="user.photo" alt="" style="width: 80px; height: 80px; border-radius: 100%;">
        </div>
        <div @click="openMessageArea(user.id)">
          <h3>{{ user.name }}</h3>
          <span>{{ user.message }}</span>
        </div>
      </li>
    </ul>
  </q-page>
</template>

<script lang="ts">
import { Todo, Meta } from 'components/models';
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      users: [
        { id: 2, name: 'Camila', photo: 'https://cdn.pixabay.com/photo/2016/06/19/22/46/girl-1467820_960_720.jpg', age: 23, status: 'Disponível', isOnline: true, message: 'Oi, tudo bem ?' }
      ]
    };
  },
  methods: {
    openMessageArea(id: number): void
    {
      void this.$router.push(`/user/${id}`);
    }
  },
  setup() {
    const todos = ref<Todo[]>([
      {
        id: 1,
        content: 'ct1'
      },
      {
        id: 2,
        content: 'ct2'
      },
      {
        id: 3,
        content: 'ct3'
      },
      {
        id: 4,
        content: 'ct4'
      },
      {
        id: 5,
        content: 'ct5'
      }
    ]);
    const meta = ref<Meta>({
      totalCount: 1200
    });
    return { todos, meta };
  }
});
</script>

<style lang="scss" scoped>
.main-container-q-page
{
  width: 100%;
  height: 80vh;
}

ul
{
  width: 100%;
  height: 100%;
  list-style: none;
  padding: 0;
  li
  {
    width: 100%;
    height: 100px;
    display: flex;
    flex-direction: row;
    padding: 0.5rem 0.5rem;
    border-bottom: 1px solid rgba(0,0,0,0.3);
    div:nth-child(1)
    {
      width: 120px;
      height: 80px;
    }
    div:nth-child(2)
    {
      width: inherit;
      height: 80px;
      box-sizing: border-box;
      -webkit-box-sizing: border-box;
      -moz-box-sizing: border-box;
      h3
      {
        width: 100%;
        height: 40%;
        margin-top: -10px;
        font-size: 1.5rem;
        font-weight: bold;
      }
      span
      {
        width: 100%;
        font-size: 1rem;
      }
    }
  }
}
</style>
