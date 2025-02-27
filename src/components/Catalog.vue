<template>
  <div>
    <div class="header">
      <div class="title">
        Services
      </div>
      <div>
        <button class="action">
          Add new service
        </button>
      </div>
    </div>
    <div class="search-bar">
      <input
        v-model="searchTerm"
        type="text"
        placeholder="Search"
        @keydown="resetPage"
      >
    </div>
    <div class="catalog">
      <KCard
        v-for="service in displayedServices"
        :key="service.id"
        class="card"
      >
        <template slot="title">
          <div class="card-title">
            {{ service.name }}
          </div>
        </template>
        <template slot="body">
          <div class="card-body">
            {{ service.description }}
          </div>
          <div class="card-footer">
            <div class="card-footer-badge">
              {{ service.versions.length }}
            </div>
            <b>Versions</b>
          </div>
        </template>
      </KCard>
    </div>
    <div class="paging">
      <button
        class="arrow"
        :disabled="isFirstPage"
        @click="previousPage"
      >
        <img
          class="reversed"
          height="44px"
          width="44px"
          :src="
            isFirstPage
              ? `${publicPath}assets/Grey-Arrow.svg`
              : `${publicPath}assets/Active-Arrow.svg`
          "
        >
      </button>
      <div class="directions">
        {{ pagingDirections }}
      </div>

      <button
        class="arrow"
        :disabled="isLastPage"
        @click="nextPage"
      >
        <img
          height="44px"
          width="44px"
          :src="
            isLastPage
              ? `${publicPath}assets/Grey-Arrow.svg`
              : `${publicPath}assets/Active-Arrow.svg`
          "
        >
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import KCard from '@kongponents/kcard'
import axios from 'axios'
import Service from '../interfaces/Service'

const ITEM_COUNT = 12

export default Vue.extend({
  name: 'Catalog',
  components: {
    KCard
  },
  data (): {
    services: Service[];
    searchTerm: string;
    page: number;
    publicPath: string;
    } {
    return {
      services: [],
      searchTerm: '',
      page: 1,
      publicPath: process.env.BASE_URL
    }
  },
  computed: {
    searchServices (): Service[] {
      return this.services.filter(this.containsSearchTerm)
    },
    displayedServices (): Service[] {
      return this.searchServices.slice(this.pagingFrom, this.pagingTo)
    },
    pagingFrom (): number {
      return ITEM_COUNT * (this.page - 1)
    },
    pagingTo (): number {
      return ITEM_COUNT * this.page
    },
    pagingDirections (): string {
      return `${this.pagingFrom + 1} - ${Math.min(
        this.pagingTo,
        this.searchServices.length
      )} of ${this.searchServices.length}`
    },
    isFirstPage (): boolean {
      return this.page === 1
    },
    isLastPage (): boolean {
      return ITEM_COUNT * this.page >= this.searchServices.length
    }
  },
  mounted () {
    this.fetchServices()
  },
  methods: {
    async fetchServices () {
      const res = await axios.get('/api/service_packages')

      this.services = res.data
    },
    nextPage () {
      if (!this.isLastPage) this.page += 1
    },
    previousPage () {
      if (!this.isFirstPage) this.page -= 1
    },
    resetPage () {
      this.page = 1
    },
    containsSearchTerm ({ name, description }: Service): boolean {
      return (
        name.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
        description.toLowerCase().includes(this.searchTerm.toLowerCase())
      )
    }
  }
})
</script>

<style lang="scss">
.header {
  margin-top: 60px;

  width: 100%;
  display: flex;
  justify-content: space-between;

  @media (max-width: 1000px) {
    margin-top: 20px;
  }
  @media (max-width: 500px) {
    flex-direction: column;
  }

  .title {
    font-weight: bold;
    font-size: 24px;
    line-height: 28px;

    color: #0a2b66;
  }

  .action {
    padding: 8px 12px;
    font-size: 16px;

    color: #ffffff;

    background: #1456cb;
    border: 0;
    border-radius: 3px;

    cursor: pointer;

    &:active {
      transform: translateY(1px);
      filter: saturate(150%);
    }
  }
}

.search-bar {
  margin-top: 24px;
  text-align: left;

  input {
    background: url(/assets/Magnifying_Glass.svg) no-repeat scroll 10px;
    padding: 10px 10px 10px 34px;
    width: calc(25% - 44px - 24px);

    @media (max-width: 1000px) {
      width: calc(100% - 44px);
    }

    font-size: 16px;
    color: rgba(0, 0, 0, 0.45);

    border: 1px solid rgba(0, 0, 0, 0.1);
    border-radius: 3px;
  }
}

.catalog {
  margin-top: 24px;
  width: 100%;
  display: grid;
  grid-template-columns: repeat(4, 1fr);

  @media (max-width: 1000px) {
    grid-template-columns: repeat(3, 1fr);
  }

  @media (max-width: 700px) {
    grid-template-columns: repeat(2, 1fr);
  }

  @media (max-width: 500px) {
    grid-template-columns: repeat(1, 1fr);
  }

  text-align: left;
  column-gap: 28px;
  row-gap: 32px;

  .card {
    padding: 24px 14px;
    min-width: 0;
    min-height: 0;

    &:hover {
      border: 1px solid #A6C6FF;
      cursor: pointer;
    }

    &-title {
      font-size: 16px;
      font-weight: bold;
      color: #1456cb;

      max-width: 100%;
      text-overflow: ellipsis;
      white-space: nowrap;
      overflow: hidden;
    }

    &-body {
      font-size: 14px;
      color: rgba(0, 0, 0, 0.45);

      display: -webkit-box;
      -webkit-line-clamp: 4;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }

    &-footer {
      margin-top: 18px;
      font-size: 13px;

      &-badge {
        font-weight: bold;
        line-height: 1;

        /* blue-500 */

        color: #1456cb;
        display: inline-block;
        border-radius: 40px;
        border: 1px solid #d9e7ff;
        padding: 2px 10px;
        margin-right: 5px;
      }
    }
  }
}

.paging {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 48px;

  margin-bottom: 82px;

  .arrow {
    display: block;
    background: none;
    border: none;
    cursor: pointer;

    &:active {
      transform: translateY(1px);
      filter: saturate(150%);
    }

    .reversed {
      -webkit-transform: scaleX(-1);
      transform: scaleX(-1);
    }
  }

  .directions {
    padding: 0 48px;
    font-size: 16px;
    color: rgba(0, 0, 0, 0.45);
    opacity: 0.7;
  }
}
</style>
