```
import { Button, DatePicker, Input, Select } from 'antd';

import * as React from 'react';

import * as moment from 'moment';

import locale from 'antd/lib/date-picker/locale/zh_CN';

import './index.scss';

import { helpers } from '@utils/index';

const Option = Select.Option;

const dateFormat = 'YYYY-MM-DD';

interface Istate {
  selectData: any;
  searchParams: any;
  [random: string]: any;
}

interface Irow {
  title: string;
  dataIndex: string;
  renderSearch?: (item: any, state: any, search: () => void) => void;
  [random: string]: any;
}
interface IProps {
  rowData: Irow[];
  handleSearch: (params: any) => void;
  dateKeys?: string[]; // 选择时间的字段
  defaultValue?: Istate; // 默认值
  size?: string;
  selectKeys?: string[]; // 属于单选下拉框的key,
  mulSelectKeys?: string[]; // 属于单选下拉框的key
  actions?: any;
  selectMap?: any; // 单选下拉数据;
  multipleSelectMap?: any; // 多选下拉数据;
  isShowResetBtn?: boolean;
}

export default class App extends React.Component<IProps, Istate> {
  private size: any = 'default';

  constructor(props: IProps) {
    super(props);
    const { selectMap, multipleSelectMap } = props;
    const selectData: any = {};
    if (selectMap) {
      Object.keys(selectMap).forEach((key: string) => {
        const values = selectMap[key];
        if (Object.prototype.toString.call(values) === '[object Object]') {
          selectData[key] = this.objToArr(values);
        } else {
          selectData[key] = values;
        }
      });
    }

    if (multipleSelectMap) {
      Object.keys(multipleSelectMap).forEach((key: string) => {
        const values = multipleSelectMap[key];
        if (Object.prototype.toString.call(values) === '[object Object]') {
          selectData[key] = this.objToArr(values);
        } else {
          selectData[key] = values;
        }
      });
    }
    this.state = {
      searchParams: {},
      selectData
    };
  }

  public componentWillMount() {
    this.getSelectData();
  }

  public objToArr(obj: any) {
    const res: any = [];
    if (!obj) {
      return res;
    }
    Object.keys(obj).forEach((key: string) => {
      res.push({
        value: key,
        label: obj[key]
      });
    });

    return res;
  }

  public search: any = () => {
    const { handleSearch } = this.props;
    const { searchParams } = this.state;
    const params: any = Object.create(null);
    Object.keys(searchParams).forEach((key: string) => {
      if (key.indexOf('_') === -1) {
        params[key] = searchParams[key];
      }
    });
    handleSearch(helpers.filterEmptyValue(params));
  };

  public clearSearch = () => {
    this.setState({
      searchParams: {}
    });
  };

  public changeSearchValue = (item: any) => {
    return (e: any) => {
      const { dateKeys = [] } = this.props;
      const type: string = item.dataIndex;
      const { selectData } = this.state;
      let value: any;
      if (selectData[item.dataIndex]) {
        value = e;
      } else if (dateKeys.indexOf(type) > -1) {
        value = (e && e.format(dateFormat)) || undefined;
        this.setState((preState: Istate) => {
          const params = Object.assign({}, preState.searchParams, { [`${type}_value`]: e });
          return { searchParams: params };
        });
      } else {
        value = e.target.value;
      }
      this.setState((preState: Istate, preProp: IProps) => {
        const params = Object.assign({}, preState.searchParams);
        params[item.dataIndex] = value;
        return { searchParams: params };
      });
    };
  };

  public disabledDate = (currentDate: any) => {
    return moment(currentDate) >= moment();
  };

  public getSelectData() {
    const { actions, selectKeys, mulSelectKeys } = this.props;
    if (!actions || (!selectKeys && !mulSelectKeys)) {
      return;
    }
    let array: any = selectKeys;
    if (array && mulSelectKeys) {
      array = array.concat(mulSelectKeys);
    } else if (mulSelectKeys) {
      array = mulSelectKeys;
    }
    actions.get('/api/dict/getDicItemsByCode', { code: array.join(',') }).then((json: any) => {
      if (json.success && json.data) {
        this.setState((preState: Istate, preProps) => {
          const params = Object.assign({}, preState.selectData, json.data);
          return { selectData: params };
        });
      }
      return;
    });
  }

  public renderSearchType = (item: any) => {
    const {
      dateKeys = [],
      selectKeys = [],
      mulSelectKeys = [],
      selectMap = {},
      multipleSelectMap = {}
    } = this.props;
    if (item.renderSearch && typeof item.renderSearch === 'function') {
      return item.renderSearch(item, this.state, this.changeSearchValue);
    } else {
      if (selectKeys.indexOf(item.dataIndex) > -1 || selectMap[item.dataIndex]) {
        const { selectData = {} } = this.state;
        const selectList = selectData[item.dataIndex] || [];
        return (
          <Select
            value={this.state.searchParams[item.dataIndex]}
            style={{ width: '100%'}}
            onChange={this.changeSearchValue(item)}
            placeholder={'请选择内容'}
            size={this.size}
            allowClear
          >
            {selectList &&
              selectList.map((it: any) => {
                return (
                  <Option value={it.value} key={it.value}>
                    {it.label}
                  </Option>
                );
              })}
          </Select>
        );
      } else if (dateKeys.indexOf(item.dataIndex) > -1) {
        return (
          <DatePicker
            disabledDate={this.disabledDate}
            locale={locale}
            value={this.state.searchParams[`${item.dataIndex}_value`]}
            onChange={this.changeSearchValue(item)}
            size={this.size}
            style={{ width: '100%' }}
          />
        );
      } else if (mulSelectKeys.indexOf(item.dataIndex) > -1 || multipleSelectMap[item.dataIndex]) {
        const { selectData } = this.state;
        const selectList = selectData[item.dataIndex] || [];
        return (
          <Select
            value={this.state.searchParams[item.dataIndex]}
            style={{ width: '100%' }}
            onChange={this.changeSearchValue(item)}
            placeholder={'请选择内容'}
            mode="multiple"
            size={this.size}
            allowClear
            showArrow
          >
            {selectList &&
              selectList.map((it: any) => {
                return (
                  <Option value={it.value} key={it.value}>
                    {it.label}
                  </Option>
                );
              })}
          </Select>
        );
      } else {
        return (
          <Input
            placeholder="请输入搜索内容"
            style={{ width: '100%' }}
            value={this.state.searchParams[item.dataIndex]}
            onChange={this.changeSearchValue(item)}
            size={this.size}
          />
        );
      }
    }
  };

  public renderResetBnt = () => {
    const { isShowResetBtn } = this.props;
    if (isShowResetBtn) {
      return (
        <Button onClick={this.clearSearch} size={this.size} className={'fl margin_left_20'}>
          重置
        </Button>
      );
    }
    return null;
  };

  public renderSearchContent() {
    const { rowData } = this.props;
    return (
      <div className={'clearfix'}>
        {rowData &&
          rowData.map((item: any) => {
            return (
              <div key={item.dataIndex}  className={helpers.reactClassNameJoin([`row-item-${item.dataIndex}`, 'row-item', 'fl'])}>
                <span className={'label fl'}>{item.title}:</span>
                <div className={helpers.reactClassNameJoin(['fl', 'min_width_150', 'input'])}>{this.renderSearchType(item)}</div>
              </div>
            );
          })}

        <div className={'fl'}>
          <Button type="primary" onClick={this.search} size={this.size} className={'fl'}>
            查询
          </Button>
          {this.renderResetBnt()}
          {this.props.children}
        </div>
      </div>
    );
  }

  public render() {
    return <div className={'fezs-group-search-container'}>{this.renderSearchContent()}</div>;
  }
}

```