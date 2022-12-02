<template>
  <ag-grid-vue
    style="width: 1200px; height: 100vh; margin: 0 auto;"
    class="ag-theme-alpine"
    :columnDefs="columnDefs.value"
    :rowData="rowData"
    @cell-clicked="onCellClicked"
    :groupIncludeTotalFooter="true"
    >
  </ag-grid-vue>
</template>

<script>
import { AgGridVue } from "ag-grid-vue3";
import { reactive } from "vue";
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-alpine.css";
import 'ag-grid-enterprise';
import array from './data';

export default {
  name: "App",
  components: {
    AgGridVue,
  },
  setup() {
    const countryCellRenderer = (params) => {
      return `<img alt="" src="${params.value}">`;
    }
    const columnDefs = reactive ( {
      value: [
    {
      headerName: 'GROUP1',
      children: [
        {
          field: 'val6',
          headerName: 'Col1',
          cellRenderer: countryCellRenderer,
        },
        { field: 'val1', headerName: 'Col2' },
        {
          field: 'val2',
          headerName: 'Col3',
          aggFunc: 'sum',
          valueFormatter: (param) => `${param.value} кг`,
        },
      ],
    },
    {
      headerName: 'GROUP2',
      children: [
        { columnGroupShow: 'open', field: 'val4' },
        {
          field: 'val5',
          colId: 'val4&val5',
          headerName: 'Col4',
          valueGetter: (param) => param.data.val4 + param.data.val5,
          aggFunc: 'avg',
        },
        {
          field: 'val7',
          headerName: 'Col5',
          cellStyle: { fontWeight: 'bold', textDecoration: 'underline' },
        },
        { field: 'val8', headerName: 'Col6', filter: true},
      ],
    },
  ]});
    const  rowData = reactive(array);
    return {
      columnDefs,
      rowData,
      onCellClicked: (params) => console.log(JSON.stringify(params.data)),
    };
  },
};

</script>
