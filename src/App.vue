<template>
  <div id="app">
    <button @click="show = true">Click Me</button>
    <Modal :modalShow="show" @close="show = false">
      <div class="transactions">
        <ul class="transactions__list">
          <li class="transactions__item"
              :key="index"
              v-for="(item, index) in transactions">
            {{ item.public_id }} - Дата создания {{ new Date(item.created_at).toLocaleDateString() }}
          </li>
        </ul>
      </div>
    </Modal>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'App',
  components: {
    Modal: () => import('./components/Modal')
  },
  data: () => ({
    show: false,
    transactions: null,
  }),

  mounted() {
    this.getTransactions();
  },
  methods: {
    async getTransactions() {
      await axios.get('https://api.stage.capusta.space/v1/cabinet/protected/transactions/page/1', {
        headers: {
          'Authorization': 'Bearer 5d370094-57a7-4476-ace4-4f29bfa43d44',
        }
      }).then(({data: {result}}) => {
        this.transactions = result.sort((a, b) => {
          return new Date(a.created_at) - new Date(b.created_at)
        }).reverse()
      }).catch(() => {
        console.warn('Something wrong!') // FIXME: Delete before push
      })

    },

  }
}
</script>

<style lang="scss">
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.transactions {

  height: 100%;
  display: flex;
  align-content: center;
  justify-content: center;
  padding: 25px;
  overflow-x: auto;
  margin-right: 30px;

  &__item {
    + .transactions__item {
      margin-top: 10px;
    }
  }
}
</style>
