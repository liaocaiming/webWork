```
import * as React from 'react';

import { Table, message } from 'antd';

import { helpers } from '../utils';

interface TopTableComponentProps {
    columns: Array<any>;
    dataSource: Array<any>;
    pagination?: any;
    orderBy?: string;
    [random: string]: any;
}

interface TopTableComponentState {
    orderBy: string;
}

class TopTableComponent extends React.PureComponent<TopTableComponentProps, TopTableComponentState>{

    constructor(props) {
        super(props);
        this.state = {
            orderBy: '',
        };
        this.tableOrderBy = this.tableOrderBy.bind(this);
        this.setRank = this.setRank.bind(this);
        this.modifyColData = this.modifyColData.bind(this);
    }

    componentWillMount() {
        this.modifyColData(this.props);
    }

    componentDidMount() {

    }

    // shouldComponentUpdate(nextProps, nextState) {
    // if (nextState.orderBy) {
    //     return this.tableOrderBy(nextProps, nextState.orderBy);
    // }
    // else {
    //     return this.tableOrderBy(nextProps, nextProps.orderBy);
    // }
    // }

    componentWillReceiveProps(nextProps, nextContext){
        this.modifyColData(nextProps);
    }
    

    componentWillUpdate(nextProps, nextState) {
        const { dataSource=[] } = nextProps;
        // this.setRank(dataSource);
    }

    //表格行数据排序,决定组件是否重绘
    tableOrderBy(nextProps, orderBy) {
        const { dataSource=[] } = nextProps;
        let reg = /\￥|\$/g;

        if (!orderBy) {
            this.setRank(dataSource);
            return true;
        }

        if (dataSource.length < 1) {
            return false;
        }
        if (!dataSource[0].hasOwnProperty(orderBy)) {
            message.warning(`specific property \'${orderBy}\' is not exist in this table row data.`);
            return false;
        }
        if (orderBy !== this.state.orderBy) {
            dataSource.sort((preRow, nextRow) => {
                if (typeof preRow[orderBy] === 'number') {
                    return nextRow[orderBy] - preRow[orderBy];
                }
                else {
                    return helpers.replaceCurrencySymbol((nextRow[orderBy])) - helpers.replaceCurrencySymbol(preRow[orderBy]);
                }
            });
        }
        // this.setRank(dataSource);
        return true;
    }

    //设置排行
    setRank(dataSource) {
        dataSource.map((item, index) => {
            if (!item.key) {
                return;
            }
            item.key = index + 1;
        });
    }

    //修改列数据
    modifyColData(nextProps) {
        const { columns } = nextProps;
        columns.map((item, index) => {
            if (item.currency) {
                item.render = function (text, record) {
                    return (<div>{helpers.midifyCurrency(text, item.currency)}</div>);
                }
            }
        });
    }

    //修改组件状态
    changeState(nextState) {
        this.setState(nextState);
    }

    render() {
        const { dataSource, columns } = this.props;
        return (<Table columns={columns}
            dataSource={dataSource}
            {...this.props}
        >
        </Table>);
    }

}

export default TopTableComponent;

```