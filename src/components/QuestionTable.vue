<template>
<DataTable
  :value="questions"
  :paginator="true"
  :filters="filters"
  :rows="25"
  :rowsPerPageOptions="[10,25,50]"
  :rowHover="true"
  v-model:selection="selected"
  dataKey="id"
  currentPageReportTemplate="Showing {first} to {last} of {totalRecords} questions"
>
      <template #header>
        <div class="table-header">
            Questions
            <span class="p-input-icon-left">
                <i class="pi pi-search" />
                <InputText v-model="filters['global']" placeholder="Global Search" />
            </span>
        </div>
    </template>
    <Column field="id" header="ID" :sortable="true" headerStyle="width: 6em">
                <template #filter>
            <MultiSelect v-model="filters['id']" :options="groups" optionLabel="name" optionValue="id" placeholder="All" class="p-column-filter">
            </MultiSelect>
        </template>
    </Column>
    <Column field="question" header="Question" :sortable="true" filterMatchMode="contains">
        <template #filter>
            <InputText type="text" v-model="filters['question']" class="p-column-filter" placeholder="Search question text"/>
        </template>
    </Column>
    <Column field="answer" header="Answer" :sortable="true">
            <template #filter>
            <InputText type="text" v-model="filters['answer']" class="p-column-filter" placeholder="Search answer text"/>
        </template>

    </Column>
</DataTable>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
import MultiSelect from 'primevue/multiselect';
import InputText from 'primevue/inputtext';
import generalQuestions from '@/data/general.json';
import generalGroups from '@/data/groups.json';


export default defineComponent({
  name: 'QuestionTable',
  components: {
    DataTable,
    Column,
    InputText,
    MultiSelect,
  },
  data: () => ({
    selected: null,
    filters: {}
  }),
  computed: {
    groups() {
      return Object.entries(generalGroups).map(([id, name]) => ({ id, name }))
    },
    questions() {
      return generalQuestions.map(q => ({
        ...q,
        answer: q.answers[q.correct],
      }))

    }
  }
});
</script>

<style lang="scss">
::v-deep(.p-paginator) {
    .p-paginator-current {
        margin-left: auto;
    }
}

::v-deep(.p-progressbar) {
    height: .5rem;
    background-color: #D8DADC;

    .p-progressbar-value {
        background-color: #607D8B;
    }
}

.table-header {
    display: flex;
    justify-content: space-between;
}

::v-deep(.p-datepicker) {
    min-width: 25rem;

    td {
        font-weight: 400;
    }
}

::v-deep(.p-datatable.p-datatable-customers) {
    .p-datatable-header {
        padding: 1rem;
        text-align: left;
        font-size: 1.5rem;
    }

    .p-paginator {
        padding: 1rem;
    }

    .p-datatable-thead > tr > th {
        text-align: left;
    }

    .p-datatable-tbody > tr > td {
        cursor: auto;
    }

    .p-dropdown-label:not(.p-placeholder) {
        text-transform: uppercase;
    }
}

/* Responsive */
.p-datatable-customers .p-datatable-tbody > tr > td .p-column-title {
    display: none;
}

@media screen and (max-width: 960px) {
    ::v-deep(.p-datatable) {
        &.p-datatable-customers {
            .p-datatable-thead > tr > th,
            .p-datatable-tfoot > tr > td {
                display: none !important;
            }

            .p-datatable-tbody > tr {
                border-bottom: 1px solid var(--layer-2);

                > td {
                    text-align: left;
                    display: block;
                    border: 0 none !important;
                    width: 100% !important;
                    float: left;
                    clear: left;
                    border: 0 none;

                    .p-column-title {
                        padding: .4rem;
                        min-width: 30%;
                        display: inline-block;
                        margin: -.4rem 1rem -.4rem -.4rem;
                        font-weight: bold;
                    }

                    .p-progressbar {
                        margin-top: .5rem;
                    }
                }
            }
        }
    }
}

</style>
