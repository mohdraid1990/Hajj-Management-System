<template>
  <div class="app">
    <div class="filters">
      <div class="search-box">
        <input
          type="text"
          v-model="searchQuery"
          placeholder="Enter id or  name ..."
          :class="{ 'hide-icon': searchQuery }"
        />
      </div>

      <div class="select-filters">
        <div class="custom-select">
          <select
            v-model="selectedCamp"
            @focus="rotateArrow('camp')"
            @blur="resetArrow('camp')"
          >
            <option value="">All Camps</option>
            <option v-for="camp in camps" :key="camp" :value="camp">
              Camp {{ camp }}
            </option>
          </select>
          <span class="arrow" :class="{ rotate: isCampArrowRotated }"></span>
        </div>

        <div class="custom-select">
          <select
            v-model="selectedTent"
            @focus="rotateArrow('tent')"
            @blur="resetArrow('tent')"
          >
            <option value="">All Tents</option>
            <option v-for="tent in tents" :key="tent" :value="tent">
              Tent {{ tent }}
            </option>
          </select>
          <span class="arrow" :class="{ rotate: isTentArrowRotated }"> </span>
        </div>
      </div>
    </div>

    <div class="content-wrapper">
      <HajjList :data="paginatedData" @view-graphic="showGraphic" />

      <div v-if="selectedView" class="graphic-view">
        <h3>{{ selectedView.type }} View</h3>
        <div class="graphic-placeholder">
          <img
            :src="'/images/' + selectedView.type.toLowerCase() + '.png'"
            :alt="selectedView.type + ' view'"
          />
          <p>{{ selectedView.type }} {{ selectedView.value }}</p>
        </div>
      </div>
    </div>

    <div class="pagination">
      <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
    </div>
  </div>
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Arial", sans-serif;
}

.app {
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;

  .filters {
    display: flex;
    flex-direction: column;
    gap: 20px;
    margin-bottom: 30px;
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

    @media (min-width: 768px) {
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
    }

    .search-box {
      flex: 1;

      input {
        width: 100%;
        padding: 12px;
        border: 1px solid #A27852;
        border-radius: 4px;
        font-family: Kumbh Sans;
font-size: 15px;
font-weight: 500;
line-height: 18.6px;
        transition: all 0.3s ease;
        background-image: url(../src/assets/Vector.svg);
        background-repeat: no-repeat;
        background-position: 20px;

        &:focus {
          outline: none;
          border-color: #4caf50;
          box-shadow: 0 0 0 2px rgba(76, 175, 80, 0.2);
        }

        &.hide-icon {
          background-image: none;
        }

        &::placeholder {
          padding: 40px;
          font-family: Kumbh Sans;
font-size: 15px;
font-weight: 400;
line-height: 18.6px;



        }
      }
    }

    .select-filters {
      display: flex;
      gap: 15px;

      .custom-select {
        position: relative;
        min-width: 150px;

        select {
          width: 100%;
          padding: 12px;
          border: 1px solid #A27852;
          border-radius: 8px;

        font-family: Kumbh Sans;
font-size: 14px;
font-weight: 500;
line-height: 17.36px;

text-underline-position: from-font;
text-decoration-skip-ink: none;

          background-color: white;
          cursor: pointer;
          appearance: none; 
          transition: all 0.3s ease;

          &:focus {
            outline: none;
            border-color: #4caf50;
            box-shadow: 0 0 0 2px rgba(76, 175, 80, 0.2);
          }

          &:hover {
            border-color: #4caf50;
          }
        }

        .arrow {
          position: absolute;
          top: 50%;
          right: 10px;
          transform: translateY(-50%);
          width: 0;
          height: 0;
          border-left: 5px solid transparent;
          border-right: 5px solid transparent;
          border-top: 5px solid #666;
          transition: transform 0.3s ease;

          &.rotate {
            transform: translateY(-50%) rotate(180deg);
          }
        }
      }
    }
  }

  .content-wrapper {
    display: grid;
    grid-template-columns: 1fr auto;
    gap: 20px;
    align-items: start;

    @media (max-width: 1024px) {
      grid-template-columns: 1fr;
    }
  }

  .graphic-view {
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    width: 300px;
    position: sticky;
    top: 20px;

    @media (max-width: 1024px) {
      width: 100%;
      position: static;
    }

    h3 {
      margin-bottom: 15px;
      color: #2c3e50;
    }

    .graphic-placeholder {
      background: #f8f9fa;
      border-radius: 4px;
      padding: 15px;
      text-align: center;

      img {
        max-width: 100%;
        height: auto;
        margin-bottom: 10px;
      }

      p {
        color: #666;
        font-weight: 500;
      }
    }
  }

  .pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
    margin-top: 30px;
    padding: 20px;
    background: white;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);

    button {
      padding: 10px 20px;
      border: none;
      background-color: #4caf50;
      color: white;
      border-radius: 4px;
      cursor: pointer;
     
      transition: all 0.3s ease;
      font-family: Kumbh Sans;
      font-size: 18px;
      font-weight: 400;
      line-height: 22.32px;

      text-underline-position: from-font;
      text-decoration-skip-ink: none;

      &:disabled {
        background-color: rgb(162, 75, 75);
        cursor: not-allowed;
      }

      &:hover:not(:disabled) {
        background-color: #455aa0;
        transform: translateY(-1px);
      }
    }

    span {
      font-family: Kumbh Sans;
font-size: 18px;
font-weight: 300;
line-height: 22.32px;

text-underline-position: from-font;
text-decoration-skip-ink: none;

      color: #666;
      
    }
  }
}
</style>

<script setup>
import { ref, computed } from "vue";
import HajjList from "./components/HajjList.vue";
import { hajjData } from "./data/hajjData";

const searchQuery = ref("");
const selectedCamp = ref("");
const selectedTent = ref("");
const currentPage = ref(1);
const itemsPerPage = 10;
const selectedView = ref(null);
const isCampArrowRotated = ref(false);
const isTentArrowRotated = ref(false);

const camps = computed(() => {
  const uniqueCamps = new Set(hajjData.map((item) => item.campNumber));
  return Array.from(uniqueCamps).sort();
});

const tents = computed(() => {
  const uniqueTents = new Set(hajjData.map((item) => item.tentNumber));
  return Array.from(uniqueTents).sort();
});

const prefixMatch = (text, query) => {
  const words = text.toLowerCase().split(" ");
  const queryWords = query.toLowerCase().split(" ");

  return queryWords.every((queryWord) =>
    words.some((word) => word.startsWith(queryWord))
  );
};

const filteredData = computed(() => {
  return hajjData.filter((item) => {
    const searchWords = searchQuery.value.trim();
    const matchesSearch =
      !searchWords ||
      item.hajjId.toString().includes(searchWords) ||
      prefixMatch(item.name, searchWords);
    const matchesCamp = !selectedCamp.value || item.campNumber === selectedCamp.value;
    const matchesTent = !selectedTent.value || item.tentNumber === selectedTent.value;
    return matchesSearch && matchesCamp && matchesTent;
  });
});

const paginatedData = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage;
  const end = start + itemsPerPage;
  return filteredData.value.slice(start, end);
});

const totalPages = computed(() => {
  return Math.ceil(filteredData.value.length / itemsPerPage);
});

const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};

const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};

const showGraphic = (type, value) => {
  selectedView.value = { type, value };
};

const rotateArrow = (type) => {
  if (type === "camp") {
    isCampArrowRotated.value = true;
  } else if (type === "tent") {
    isTentArrowRotated.value = true;
  }
};

const resetArrow = (type) => {
  if (type === "camp") {
    isCampArrowRotated.value = false;
  } else if (type === "tent") {
    isTentArrowRotated.value = false;
  }
};
</script>
