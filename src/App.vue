<template>
  <div id="app">
    <div
      v-for="category in categories"
      :key="category.id"
      @drop="onDrop($event, category.id)"
      class="droppable"
      @dragover.prevent
      @dragenter.prevent
    >
      <h4>{{ category.title }}</h4>
      <div
        v-for="item in items.filter(x => x.categoryId == category.id)"
        :key="item.id"
        @dragstart="onDragStart($event, item)"
        class="draggble"
        draggable="true"
      >
        <h5>{{ item.title }}</h5>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      categories: [
        {
          id: 0,
          title: "Cars",
        },
        {
          id: 1,
          title: "Animals",
        },
      ],
      items: [
        {
          id: 0,
          title: "Audi",
          categoryId: 0,
        },
        {
          id: 1,
          title: "BMW",
          categoryId: 0,
        },
        {
          id: 2,
          title: "Cat",
          categoryId: 1,
        }
      ],
    };
  },
  methods: {
    onDragStart(e, item) {
      e.dataTransfer.dropEffect = 'move'
      e.dataTransfer.effectAllowed = 'move'
      e.dataTransfer.setData('itemId', String(item.id))
    },
    onDrop(e, categoryId) {
      const itemId = parseInt(e.dataTransfer.getData('itemId'))
      this.items.map(x => {
        if (x.id == itemId){
          x.categoryId = categoryId
          return x
        }
      })
    }
  },
};
</script>

<style lang="scss">
#app {
  .droppable {
    padding: 15px;
    border-radius: 5px;
    background: #2c3e50;
    margin-bottom: 10px;

    h4 {
      color: white;
    }
  }
  .draggble {
    background: white;
    padding: 5px;
    border-radius: 5px;
    margin-bottom: 5px;

    h5 {
      margin: 0;
    }
  }
}
</style>
