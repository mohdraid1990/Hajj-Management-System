<script setup>
defineProps({
  data: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["viewGraphic"]);
</script>

<template>
  <div class="hajj-list">
    <div class="list-header">
      <div class="header-item">ID</div>
      <div class="header-item">Name</div>
      <div class="header-item">Camp No.</div>
      <div class="header-item">Tent No.</div>
      <div class="header-item">Bed No.</div>
      <div class="header-item">Passport No.</div>
    </div>

    <div class="list-body">
      <div v-for="item in data" :key="item.hajjId" class="list-row">
        <div class="list-item">{{ item.hajjId }}</div>
        <div class="list-item">{{ item.name }}</div>
        <div class="list-item">
          {{ item.campNumber }}
          <button
            class="view-btn"
            @click="emit('viewGraphic', { type: 'Camp', value: item.campNumber })"
          >
            <img src="../assets/map.svg" alt="" />
          </button>
        </div>
        <div class="list-item">
          {{ item.tentNumber }}
          <button
            class="view-btn"
            @click="emit('viewGraphic', { type: 'Tent', value: item.tentNumber })"
          >
            <img src="../assets/map.svg" alt="" />
          </button>
        </div>
        <div class="list-item">
          {{ item.bedNumber }}
          <button
            class="view-btn"
            @click="emit('viewGraphic', { type: 'Bed', value: item.bedNumber })"
          >
            <img src="../assets/map.svg" alt="" />
          </button>
        </div>
        <div class="list-item">{{ item.passportNumber }}</div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.hajj-list {
  background: #ffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow-x: auto;

  .list-header {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    background-color: #fff1de;
    padding: 15px;
    gap: 10px;
    position: sticky;
    top: 0;
    z-index: 1;

    .header-item {
      color: #232323;
      font-family: Kumbh Sans;
      font-size: 18px;
      font-weight: 600;
      line-height: 22.32px;

      text-underline-position: from-font;
      text-decoration-skip-ink: none;

      text-align: center;
      text-transform: uppercase;
      letter-spacing: 0.5px;
     
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
    }
  }

  .list-body {
    .list-row {
      display: grid;
      grid-template-columns: repeat(6, 1fr);
      padding: 12px 15px;
      gap: 10px;
      border-bottom: 1px solid #C9C9C980;
      transition: all 0.3s ease;

      &:hover {
        background-color: #f5f5f5;
        transform: translateY(-1px);
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
      }

      .list-item {
        text-align: center;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        padding: 8px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        font-family: Kumbh Sans;
        font-size: 16px;
        font-weight: 500;
        line-height: 19.84px;

        text-underline-position: from-font;
        text-decoration-skip-ink: none;
      }
    }
  }
}

.view-btn {
  background: none;
  border: none;
  color: inherit;
  cursor: pointer;
  padding: 4px;
  opacity: 0.7;
  transition: opacity 0.3s ease;

  &:hover {
    opacity: 1;
  }

  i {
    font-size: 10px;
  }
}

@media (max-width: 768px) {
  .hajj-list {
    .list-header,
    .list-row {
      grid-template-columns: repeat(3, 1fr);
    }

    .header-item:nth-child(n + 4),
    .list-item:nth-child(n + 4) {
      display: none;
    }
  }
}
</style>
