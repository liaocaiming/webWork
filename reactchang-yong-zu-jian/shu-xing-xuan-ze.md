```
import * as React from 'react';

import { Checkbox, Tree } from 'antd';

const TreeNode = Tree.TreeNode;

interface ITreeItem {
  key?: string;
  title?: string;
  value?:string;
  label?: string;
  disabled?:boolean;
  children?: ITreeItem[];
  [random:string]: any;
}

interface IProps {
  treeGroupArray: ITreeItem[];
  required?: boolean;
  requiredMessage?: string;
  isShowAllSelectBtn?:boolean;
  onChange?: (parms: string[]) => void;
  defaultCheckedKeys?: string[]
  [random: string]: any;
}

interface IState {
  checkedAll: boolean;
  indeterminate: boolean;
  checkedKeys: any;
  [random: string]: any;
}

export default class App extends React.PureComponent<IProps, IState> {
  public static defaultProps: IProps = {
    treeGroupArray: [],
    required: true,
    requiredMessage: '至少选择一项'
  };

  public AllKeyArr: string[] = []; // 选择权限的ID

  constructor(props: IProps) {
    super(props);
    this.AllKeyArr = this.getAllkeyArr(this.props.treeGroupArray);
    this.state = {
      checkedAll: false,
      indeterminate: false,
      checkedKeys: props.defaultCheckedKeys || [],
      autoExpandParent: false
    };
  }

  // 渲染树结构
  public renderTreeNodes = (data: any) => {
    return data.map((item: ITreeItem, index: number) => {
      if (item.children) {
        return (
          <TreeNode title={item.title || item.label} key={item.key || item.value} {...item}>
            {this.renderTreeNodes(item.children)}
          </TreeNode>
        );
      }
      return <TreeNode {...item} key={item.key || item.value} title={item.title || item.label} />;
    });
  };

  // 全选按钮
  public onSelectedAll = (e: any) => {
    this.setState({
      checkedKeys: e.target.checked ? this.AllKeyArr : [],
      checkedAll: e.target.checked,
      indeterminate: false
    });
    return;
  };

  public getAllkeyArr = (arr: any) => {
    const res: string[] = [];
    if (!arr || (arr && arr.length === 0)) {
      return res;
    }
    const getAllkeyArr = (array: any) => {
      array.map((item: any) => {
        if (item.children) {
          res.push(item.key);
          getAllkeyArr(item.children);
        } else {
          res.push(item.key);
        }
      });
    };

    getAllkeyArr(arr);
    return res;
  };

  // 张开
  public onExpand = (expandedKeys: any) => {
    this.setState({
      expandedKeys,
      autoExpandParent: false
    });
  };

  // 复选框
  public onCheck = (checkedKeys: any) => {
    const { onChange } = this.props;
    const indeterminate = checkedKeys.length !== this.AllKeyArr.length && checkedKeys.length;
    if (onChange) {
      onChange(checkedKeys);
    }
    this.setState({
      checkedKeys,
      checkedAll: checkedKeys.length === this.AllKeyArr.length,
      indeterminate
    });
  };

  // // 选中树
  // public onSelect = (selectedKeys:any, info:any) => {
  //   this.setState({ selectedKeys, checkedKeys: selectedKeys });
  // };

  public componentWillReceiveProps(nextProps:IProps) {
    if (this.props.defaultCheckedKeys !== nextProps.defaultCheckedKeys) {
      this.setState({
        checkedKeys: nextProps.defaultCheckedKeys
      })
    }
  }

  public renderAllSelectOrNot = () => {
    const {requiredMessage, disabled, required, isShowAllSelectBtn } = this.props;
    const { checkedKeys, indeterminate } = this.state;
    if (!isShowAllSelectBtn) {
      return;
    }
    return (
      <div>
        <Checkbox
          onChange={this.onSelectedAll}
          checked={this.state.checkedAll}
          indeterminate={indeterminate}
        >
          全部
        </Checkbox>
        {!disabled &&
          required &&
          checkedKeys.length < 1 && <span style={{ color: 'red' }}>{requiredMessage}</span>}
      </div>
    );
  };

  public render() {
    const { treeGroupArray } = this.props;
    const { expandedKeys, autoExpandParent, checkedKeys, selectedKeys } = this.state;
    return (
      <div id={'tree'}>
        {this.renderAllSelectOrNot()}
        <Tree
          checkable
          showLine={true}
          onExpand={this.onExpand}
          expandedKeys={expandedKeys}
          autoExpandParent={autoExpandParent}
          onCheck={this.onCheck}
          // onSelect={this.onSelect}
          checkedKeys={checkedKeys} 
          selectedKeys={selectedKeys}
          {...this.props}
        >
          {this.renderTreeNodes(treeGroupArray)}
        </Tree>
      </div>
    );
  }

}

```