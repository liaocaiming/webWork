```js
import * as React from 'react';

import { DatePicker, Form, Input, Select } from 'antd';

import { WrappedFormUtils } from 'antd/lib/form/Form.d';

const FormItem = Form.Item;
const Option = Select.Option;
const TextArea = Input.TextArea;

const map = {
  select: Select,
  input: Input,
  textArea: TextArea,
  datePicker: DatePicker,
};

export interface IList {
  value: string;
  label: string;
}
let key = 0;
type type = 'select' | 'input' | 'textArea' | 'datePicker';
export interface IFormItem {
  id?: string; // id dataIndex key 传一个就行, 这个是为了兼容; 用dataIndex;
  dataIndex?: string;
  key?: string;
  type?: type;
  isShow?: boolean // 是否显示
  rules?: any[];
  label?: string; // label title 传一个就行, 这个是为了兼容, 用title;
  title?: string;
  initialValue?: any;
  list?: IList[];
  render?: (formItem: IFormItem) => any;
  eleAttr?: { placeholder?: any; disabled?: boolean; style?: any; [random: string]: any }; // 元素的属性
}

interface Iprops {
  formItem: IFormItem;
  formItemLayout?: {
    labelCol: any;
    wrapperCol: any;
  };
  getFieldDecorator: WrappedFormUtils["getFieldDecorator"];
  [random: string]: any;
}

export default class App extends React.Component<Iprops, any> {
  constructor(props: Iprops) {
    super(props);
  }

  public renderFormEle(formItem: IFormItem) {
    if (formItem.render && typeof formItem.render === 'function') {
      return formItem.render(formItem);
    }
    const { eleAttr = {} } = formItem;

    if (formItem.type === 'select' && formItem.list) {
      return (
        <Select {...eleAttr}>
          {formItem.list &&
            formItem.list.map((item: any) => {
              return (
                <Option key={item.value} value={item.value}>
                  {item.label}
                </Option>
              );
            })}
        </Select>
      );
    }

    if (formItem.type && map[formItem.type]) {
      const Item:any = map[formItem.type];
      return <Item {...eleAttr} />;
    }
    return <Input {...eleAttr} />;
  }

  public render() {
    const { formItemLayout, formItem, getFieldDecorator } = this.props;

    const layout = Object.assign({}, {
      labelCol: {
        span: 6,
      },
      wrapperCol: {
        span: 18,
      },
    }, formItemLayout);
    key++

    if (formItem.isShow === false) {
      return null;
    }

    return (
      <div>
        <FormItem label={formItem.label || formItem.title}  {...layout}>
          {getFieldDecorator(formItem.id || formItem.dataIndex || formItem.key ||  `${key}`, {
            initialValue: formItem.initialValue,
            rules: formItem.rules,
          })(this.renderFormEle(formItem))}
        </FormItem>
      </div>
    );
  }
}

```