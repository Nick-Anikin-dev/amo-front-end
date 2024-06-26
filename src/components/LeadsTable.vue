<template>
  <a-table :loading="loading" :columns="columns" :data-source="data" bordered :pagination="false">
    <template #title >
       <h2>Тестовое задание</h2>
       <a-input-search
         v-model:value="searchQuery"
         placeholder="Search..."
         style="width: 200px"
      />
      </template>
      
    <template #expandedRowRender="{ record }">
      <h4>Контакты:</h4>
      <div v-for="contact in record.contacts" :key="contact.id">
        <p style="margin: 0">
         Имя: {{ contact.name }}
        </p>
         <p  v-for="field in contact.customFieldsValues" :key="field.name">
              <span>{{field.name}}</span> : {{ field.values.join(',') }}
        </p>
        
    </div>
    </template>
  </a-table>
</template>

<style scoped>
th.column-money,
td.column-money {
  text-align: right !important;
}
</style>

<script>

const columns = [
  {
    title: 'Название',
    dataIndex: 'name',
  },
  {
    title: 'Бюджет',
    className: 'column-money',
    dataIndex: 'budget',
  },
  {
    title: 'Статус',
    dataIndex: 'status',
  },
  {
    title: 'Ответственный',
    dataIndex: 'responsible',
  },
  {
    title: 'Дата создания',
    dataIndex: 'createdAt',
  },
];

export default {
  name: 'LeadsTable',
  data() {
    return {
      data: [],
      columns,
      debounceTimer: null,
      searchQuery: '',
      loading: false
    };
  },
  props: {
    msg: String
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.loading = true;
      const leadsUrl = 'https://amo-back-end-682e41383993.herokuapp.com/leads';
      fetch(`${leadsUrl}?query=${this.searchQuery}`)
        .then(response => response.json())
        .then(data => {
          this.data = data?.map(lead => ({
            ...lead,
            key: lead.name,
            responsible: `${lead.responsible.name} (${lead.responsible.email})`,
            createdAt: new Date(lead.createdAt).toLocaleDateString(),
          })) || [];
        })
        .catch(error => {
          alert(`Failed to fetch leads: ${error}`);
        })
        .finally(()=>{
          this.loading = false;
        });
    },
  },

  watch: {
    searchQuery() {
      if (this.debounceTimer) {
        clearTimeout(this.debounceTimer); 
      }
      this.debounceTimer = setTimeout(() => {
        this.fetchData(); 
      }, 500); 
    }
  }
}
</script>