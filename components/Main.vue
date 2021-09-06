<template>
  <SearchForm
    :labelLength="6"
    :advanceSearchItems="advanceSearchItems"
    @onSearch="onSearch"
    isShowTab
    :tabs="tabs"
  >
  </SearchForm>
  <BasicGrid
    :columnDefs="columnDefs"
    :rowData="rowData"
    :headerHeight="24"
    :gridDefs="gridDefs"
    :pageData="pageData"
    :gridOptions="gridOptions"
    @pageChange="onPageChange"
  />
</template>
<script lang="ts">
import { computed, defineComponent, reactive, ref } from 'vue';
import { SearchForm, BasicGrid, CellCommonRenderer } from 'xiezan-ui';
import {
  IFormItem,
  ICollects,
  IKeywords,
  ITags,
  ITabPane,
} from 'xiezan-ui/lib/form/types';
import { queryList } from '@/api/reminderReply';

export default defineComponent({
  name: 'Main',
  components: {
    SearchForm,
    BasicGrid,
    /*eslint-disable */
    CellCommonRenderer,
    /*eslint-enable */
  },
  setup() {
    const pageIndex = ref(1);
    const pageSize = ref(40);
    const total = ref(0);
    const rowData = ref<any[]>([]);
    const pageData = computed(() => {
      return {
        pageIndex: pageIndex.value,
        pageSize: pageSize.value,
        totalCount: total.value,
        pageSizeOptions: ['40', '60', '80', '100', '200'],
      };
    });

    const onSearch = (val: {
      keywords: IKeywords;
      collects: ICollects[];
      tags: ITags;
      keywordsMap: IKeywords;
      tagsMap: ITags;
    }) => {
      const { keywords } = val;
      queryList(keywords).then((res: any) => {
        if (res.code === 0) {
          res.data.resultList.forEach((item: any, index: any) => {
            item.serialNum = index + 1;
          });
          rowData.value = res.data.resultList;
          pageIndex.value = res.data.pageIndex;
          pageSize.value = res.data.pageSize;
          total.value = res.data.pageCount;
        }
      });
    };

    const gridOptions = reactive({
      api: {},
      columnApi: {},
    });

    const gridDefs = reactive({});

    const advanceSearchItems = reactive<IFormItem[]>([
      {
        dataIndex: 'purchaseNo',
        key: 'purchaseNo',
        type: 'Input',
        value: '',
        label: '采购单号',
        placeholder: '请输入采购单号',
        istop: false,
      },
      {
        dataIndex: 'contractNo',
        key: 'contractNo',
        type: 'Input',
        value: '',
        label: '供应商合同号',
        placeholder: '请输入供应商合同号',
        istop: false,
      },
      {
        dataIndex: 'supplierNameList',
        key: 'supplierNameList',
        type: 'Input',
        value: '',
        label: '供应商',
        placeholder: '请输入供应商',
        istop: false,
      },
      {
        dataIndex: 'brandId',
        key: 'brandId',
        type: 'RangePicker',
        value: '',
        label: '最新要求货期',
        placeholder: '请选择日期范围',
        istop: false,
      },
      {
        dataIndex: 'brandIdList',
        key: 'brandIdList',
        type: 'Select',
        value: [],
        label: '品牌',
        placeholder: '请选择品牌',
        groupType: 'SearchGroup',
        istop: false,
        options: [],
        props: {
          showSearch: true,
          //filterOption: [],
        },
      },
      {
        dataIndex: 'materialNoOrModelNames',
        key: 'materialNoOrModelNames',
        type: 'MultipleSelect',
        value: [],
        label: '订货号/型号',
        placeholder: `请选择订货号/型号|多个订货号/型号可以换行隔开，最多支持输入500个订货号/型号，批量查询不支持模糊搜索，请输入完整订货号/型号名称
例如：
A9F18106`,
        istop: true,
        options: [],
        groupType: 'SearchGroup',
        props: {
          showTextArea: true,
        },
      },
      {
        dataIndex: 'orderNo',
        key: 'orderNo',
        type: 'Input',
        value: '',
        label: '订单号',
        placeholder: '请输入订单号',
        istop: false,
      },
      {
        dataIndex: 'brandId',
        key: 'brandId',
        type: 'RangePicker',
        value: '',
        label: '最新采购货期',
        placeholder: '请选择日期范围',
        istop: false,
      },
      {
        dataIndex: 'createdByList',
        key: 'createdByList',
        type: 'Input',
        value: '',
        label: '采购单制单人',
        placeholder: '请选择制单人',
        istop: false,
      },
      {
        dataIndex: 'reminderStatusList',
        key: 'reminderStatusList',
        type: 'Select',
        value: [],
        label: '催单状态',
        placeholder: '请选择催单状态',
        istop: false,
        options: [
          {
            id: '1',
            name: '已处理',
          },
          {
            id: '2',
            name: '未处理',
          },
          {
            id: '3',
            name: '已完成',
          },
        ],
      },
    ]);

    const tabs = reactive<ITabPane[]>([
      {
        key: '1',
        tabName: '全部',
        closable: false,
      },
      {
        key: '2',
        tabName: '未处理',
        closable: false,
      },
      {
        key: '3',
        tabName: '已处理',
        closable: false,
      },
    ]);

    function onPageChange(current: number, size: number) {
      console.log(current, size);
    }

    const columnDefs = reactive([
      {
        suppressColumnsToolPanel: true,
        suppressMenu: true,
        field: '',
        headerName: '',
        width: 52,
        minWidth: 52,
        headerCheckboxSelection: true,
        pinned: 'left',
        resizable: false,
        cellClass: 'ag-cell-align-center',
        checkboxSelection: true,
      },
      {
        suppressColumnsToolPanel: true,
        suppressMenu: true,
        field: 'serialNum',
        headerName: '序号',
        width: 50,
      },
      {
        suppressColumnsToolPanel: true,
        field: 'purchaseNo',
        headerName: '采购单号',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: 'contractNo',
        headerName: '供应商合同号',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: 'orderNo',
        headerName: '订单号',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '供应商',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '产品信息',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '订货号',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '型号',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '数量',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '入库数量',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '原始期望货期',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '初次采购货期',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '最新要求货期',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '货期状态（商务）',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '货期状态（采购）',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '最近一次催单人',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '采购单制单人',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '最近一次催单时间',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '最近一次处理时间',
        minWidth: 150,
      },
      {
        suppressColumnsToolPanel: true,
        field: '',
        headerName: '操作',
        width: 80,
        pinned: 'right',
        cellRendererFramework: 'CellCommonRenderer',
        cellRendererParams: {
          props: {
            type: 'btnGroup',
            btnGroup: [
              {
                btnText: '更新货期',
                click: (params: any) => {
                  console.log(params);
                },
                disabled: () => {
                  return false;
                },
              },
            ],
          },
        },
      },
    ]);

    return {
      onPageChange,
      rowData,
      pageData,
      advanceSearchItems,
      onSearch,
      gridOptions,
      gridDefs,
      columnDefs,
      tabs,
    };
  },
});
</script>
