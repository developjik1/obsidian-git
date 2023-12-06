'use client';

import React, { ReactNode, useCallback, useEffect, useMemo, useRef, useState } from 'react';

import type { TableProps } from 'antd';

import { Table } from 'antd';

import { ColumnsType } from 'antd/es/table';

import { ColumnType, RowSelectMethod } from 'antd/es/table/interface';

  

import { Box, Flex } from '../layout';

import { B3 } from '../typography';

import { Empty, PaginationProps } from '../unit';

import { ColoDoubleArrowLeft, ColoDoubleArrowRight } from '../svg';

import { Render } from './column-render';

  

import { useContentWidthHeight, useQueryParams, useTable, useWindowSize } from '../../hooks';

  

import { COLORS } from '../../styles';

  

import './css/table.css';

  

export const TABLE_SELECTION_COLUMN = Table.SELECTION_COLUMN;

export const TABLE_EXPAND_COLUMN = Table.EXPAND_COLUMN;

  

export type UnitColumnType<T> = ColumnType<T>;

export type UnitColumnsType<T> = ColumnsType<T>;

  

export type UnitScreenTableProps<T> = TableProps<T> & {

onRowClick?: (record: T, index?: number) => void;

showPagination?: boolean;

showPaginationSizeChanger?: boolean;

showNum?: boolean;

showSelect?: boolean;

selectType?: 'checkbox' | 'radio';

showExpand?: boolean;

defaultPageSize?: number;

pageSizeOptions?: number[];

emptyText?: string;

gap?: number;

};

  

export const UnitScreenTable = <T extends object>({

columns = [],

dataSource,

showPagination = false,

showPaginationSizeChanger = false,

showNum = false,

showSelect = false,

selectType = 'checkbox',

defaultPageSize = 25,

pageSizeOptions = [25, 50, 100, 1000],

showExpand = false,

virtual = true,

gap = 0,

...props

}: UnitScreenTableProps<T>) => {

const wrapperRef = useRef<HTMLDivElement>(null);

const tableRef: Parameters<typeof Table>[0]['ref'] = useRef(null);

  

const { screenTableHeight } = useContentWidthHeight();

const wrapperHeight = useMemo(() => screenTableHeight - gap, [screenTableHeight, gap]);

  

const { setQueryParams } = useQueryParams();

const {

page,

setPage,

setClickRow,

clickRowIndex,

setClickRowIndex,

clearClickRowIndex,

selectedRowKeys,

setSelectedRowKeys,

setSelectedRows,

expandedRowKeys,

setExpandedRowKeys,

} = useTable();

  

const data = useMemo(() => dataSource?.map((d, i) => ({ ...d, key: i + 1 })), [dataSource]);

  

// table props rowClassName

const rowClassName: TableProps<T>['rowClassName'] = useCallback(

(_record: T, index: number) => `colo-table-${index % 2 === 0 ? 'even' : 'odd'}-row colo-table-row-${index + 1}`,

[],

);

  

// table props scroll

const [y, setY] = useState(0);

const { width: windowWidth } = useWindowSize();

const [x, setX] = useState(0);

const { minContentWidth, contentWidth } = useContentWidthHeight();

useEffect(() => {

setX(windowWidth < 1600 ? minContentWidth : contentWidth);

}, [windowWidth, minContentWidth, contentWidth]);

  

// y set useEffect

useEffect(() => {

const headHeight = wrapperRef?.current ? wrapperRef?.current?.querySelector('.ant-table-thead')?.clientHeight || 0 : 0;

  

const paginationHeight =

showPagination && wrapperRef?.current && wrapperRef?.current?.querySelector('.ant-pagination')

? wrapperRef?.current?.querySelector('.ant-pagination')?.clientHeight || 0

: 0;

  

setY(wrapperHeight - headHeight - paginationHeight);

}, [

wrapperHeight,

wrapperRef?.current?.querySelector('.ant-table-thead')?.clientHeight,

wrapperRef?.current?.querySelector('.ant-pagination')?.clientHeight,

]);

  

const scroll: TableProps<T>['scroll'] = useMemo(

() =>

virtual

? {

x,

y,

}

: undefined,

[virtual, x, y],

);

  

useEffect(() => {

if (clickRowIndex) {

setTimeout(() => {

tableRef?.current?.scrollTo({ index: clickRowIndex });

}, 150);

  

setTimeout(() => {

clearClickRowIndex();

}, 300);

}

}, [clickRowIndex]);

  

// row select

const onSelectChange = useCallback(

(

selectedKeys: React.Key[],

selectedRows: T[],

info: {

type: RowSelectMethod;

},

) => {

if (info.type === 'all') {

if (selectedRowKeys?.length !== data?.length) {

setSelectedRowKeys(data?.map((d) => d?.key) as React.Key[]);

setSelectedRows(data as any[]);

} else {

setSelectedRowKeys([]);

setSelectedRows([]);

}

} else {

setSelectedRowKeys(selectedKeys);

setSelectedRows(selectedRows);

}

},

[selectedRowKeys, setSelectedRowKeys, setSelectedRows],

);

  

// table props rowSelection

const rowSelection: TableProps<T>['rowSelection'] = useMemo(

() =>

showSelect

? {

type: selectType,

columnWidth: 80,

selectedRowKeys,

onChange: onSelectChange,

}

: undefined,

[selectType, showSelect, selectedRowKeys, onSelectChange],

);

  

// pagination component & (page & page size) state + fn

const [pageSize, setPageSize] = useState<number>(defaultPageSize);

  

const lastPage = useMemo(() => {

const quotient = (data?.length || 0) / pageSize;

const rest = (data?.length || 0) % pageSize;

  

return quotient + (rest === 0 ? 0 : 1);

}, [data?.length, pageSize]);

  

const onShowSizeChange: PaginationProps['onShowSizeChange'] = useCallback(

(_page: number, pageSize: number) => {

setPageSize(pageSize);

},

[setPageSize],

);

const onChangePagination = useCallback(

(page: number) => {

setPage(page);

},

[setPage],

);

const itemRenderPagination = (

_current: number,

type: 'page' | 'prev' | 'next' | 'jump-prev' | 'jump-next',

originalElement: ReactNode,

) => {

const goToFirstPage = (e: React.MouseEvent<HTMLElement, MouseEvent>) => {

setPage(1);

e.stopPropagation();

};

  

const goToLastPage = (e: React.MouseEvent<HTMLElement, MouseEvent>) => {

setQueryParams(lastPage);

e.stopPropagation();

};

  

if (type === 'prev') {

return (

<Flex className="colo-table-pagination-goto-page-wrapper" align="center" gap={10}>

<Flex className="colo-table-pagination-goto-page-first" onClick={goToFirstPage}>

<ColoDoubleArrowLeft opacity={page === 1 ? 0.1 : 0.4} />

</Flex>

<Flex className="colo-table-pagination-goto-page-before">{originalElement}</Flex>

</Flex>

);

}

if (type === 'next')

return (

<Flex align="center" gap={10}>

<Flex className="colo-table-pagination-goto-page-next">{originalElement}</Flex>

<Flex className="colo-table-pagination-goto-page-end" onClick={goToLastPage}>

<ColoDoubleArrowRight opacity={page === lastPage ? 0.1 : 0.4} />

</Flex>

</Flex>

);

  

return <>{originalElement}</>;

};

  

// table props pagination

const pagintation: TableProps<T>['pagination'] = useMemo(

() =>

showPagination

? {

current: page,

onChange: onChangePagination,

className: 'colo-table-pagination',

position: ['bottomCenter'],

pageSize,

pageSizeOptions,

showSizeChanger: showPaginationSizeChanger,

onShowSizeChange,

locale: { items_per_page: '개' },

itemRender: (current: number, type: 'page' | 'prev' | 'next' | 'jump-prev' | 'jump-next', originalElement: ReactNode) =>

itemRenderPagination(current, type, originalElement),

}

: false,

[

showPagination,

page,

onChangePagination,

pageSize,

pageSizeOptions,

showPaginationSizeChanger,

onShowSizeChange,

itemRenderPagination,

props?.pagination,

],

);

  

// table expandable props

const onExpand = useCallback(

(expanded: boolean, record: any) =>

setExpandedRowKeys(expanded ? [...expandedRowKeys, record?.key] : expandedRowKeys?.filter((key) => key !== record?.key)),

[expandedRowKeys, setExpandedRowKeys],

);

  

const handleExpandAll = useCallback(() => {

if (expandedRowKeys?.length === data?.length) {

setExpandedRowKeys([]);

} else {

setExpandedRowKeys(data?.map((d) => d?.key) as React.Key[]); // Expand all rows by setting keys of all data items

}

}, [expandedRowKeys, data, setExpandedRowKeys]);

  

const isExpandedAll = useMemo(() => data?.length === expandedRowKeys?.length, [data, expandedRowKeys]);

  

const expandable: TableProps<T>['expandable'] = useMemo(

() =>

showExpand

? {

columnTitle: (

<Flex justify="center" align="center" onClick={handleExpandAll}>

<B3 color={COLORS.TEXT_COLOR} fontWeight={700}>

{isExpandedAll ? '-' : '+'}

</B3>

</Flex>

),

columnWidth: 72,

expandedRowKeys,

onExpand,

...props?.expandable,

}

: undefined,

[isExpandedAll, showExpand, expandedRowKeys, props?.expandable],

);

  

// table custom columns

const tableColumns: ColumnsType<T> = useMemo(() => {

const newColumns: ColumnsType<T> = columns.map((col) => {

if (!('render' in col)) {

return {

...col,

render: (text: any) => <Render>{text}</Render>,

align: 'center',

};

}

  

return {

...col,

align: 'center',

};

});

  

if (showNum) {

newColumns.unshift({

title: 'Num',

dataIndex: 'key',

render: (_text: number, _record: T, index: number) => <>{index + 1}</>,

width: 72,

align: 'center',

});

}

  

if (showExpand) {

newColumns.unshift(TABLE_EXPAND_COLUMN);

}

  

if (showSelect) {

newColumns.unshift(TABLE_SELECTION_COLUMN);

}

  

return newColumns;

}, [showNum, showExpand, showSelect, dataSource]);

  

const onRowClick = useCallback(

(record: T, rowIndex?: number) => {

setClickRowIndex(rowIndex as number);

setClickRow(record);

  

// pushQueryParams(pathname, {

// clickRow: JSON.stringify(record),

// });

},

[setClickRowIndex, setClickRow],

);

  

return (

<Box ref={wrapperRef} height={wrapperHeight} width={props?.scroll?.x || x - 4 || '100%'}>

<Table<T>

ref={tableRef}

className="colo-table-wrapper"

rowClassName={rowClassName}

columns={tableColumns}

dataSource={data}

scroll={scroll}

virtual={virtual}

rowSelection={rowSelection}

pagination={pagintation}

// components={{

// body: {

// cell: (props: any) => ({

// style: {

// width: props.column.width || '100%', // 각 열의 너비 설정

// },

// }),

// },

// }}

locale={{

emptyText: props?.emptyText || <Empty height={y} btnText={'버튼'} />,

}}

onRow={(record, rowIndex) => ({

onClick: (event) => {

if (

!['ant-table-row-expand-icon-cell', 'ant-table-selection-column'].includes(

(event.target as HTMLInputElement).classList[1] as string,

)

) {

return props?.onRowClick ? props?.onRowClick?.(record, rowIndex) : onRowClick(record, rowIndex);

}

},

...props?.onRow,

})}

showSorterTooltip={false}

{...props}

expandable={expandable}

/>

</Box>

);

};