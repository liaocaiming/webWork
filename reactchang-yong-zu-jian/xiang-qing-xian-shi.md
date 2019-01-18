```
import { helpers } from '@utils/index';
import { Col, Row } from 'antd';
import * as React from 'react';
interface IDetailShowItem {
  title: string;
  dataIndex: string;
  currency?: string;
  describe?: string;
  render?: (text:string, detail:any) => any;
}

interface IlabelWrapperCol {
  labelSpan?: number;
  wrapperSpan?: number;
}
interface IDetailShowComponentProps {
  rowData: IDetailShowItem[];
  detail: any;
  colSpan?: number;
  labelWrapperColObj?: IlabelWrapperCol;
}

const renderTitleContent = (item: IDetailShowItem, obj: any) => {
  if (item.currency) {
    return <span>{helpers.midifyCurrency(obj[item.dataIndex], item.currency)}</span>;
  }
  if (item.describe === 'img') {
    return <img src={obj[item.dataIndex]} width={'150'} />;
  }
  return <span>{obj[item.dataIndex]}</span>;
};
export default function(props: IDetailShowComponentProps) {
  const spanObj: IlabelWrapperCol = {
    labelSpan: 6,
    wrapperSpan: 16
  };
  const { rowData, detail, colSpan, labelWrapperColObj } = props;
  if (labelWrapperColObj) {
    Object.assign(spanObj, labelWrapperColObj);
  }
  let span = 1;
  if (colSpan) {
    span = colSpan;
  }
  const data = helpers.dyadicArray(rowData, span);
  if ((data && data.length === 0) || !detail) {
    return <span>&nbsp;</span>;
  }

  return (
    <div>
      {data.map((list: any, index: number) => {
          return (
            <Row key={index}>
              {list.map((it: any, idx: number) => {
                return (
                  <Col key={idx} span={24 / span}>
                    <Row style={{ height: '50px', lineHeight: '50px' }}>
                      <Col
                        span={spanObj.labelSpan}
                        style={{ textAlign: 'right', marginRight: '10px' }}
                      >
                        {it.title}:{' '}
                      </Col>
                      <Col span={spanObj.wrapperSpan}>{it.render ? it.render(it.dataIndex, detail) : renderTitleContent(it, detail)}</Col>
                    </Row>
                  </Col>
                );
              })}
            </Row>
          );
        })}
    </div>
  );
}

```