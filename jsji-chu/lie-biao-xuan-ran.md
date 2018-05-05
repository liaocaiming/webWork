### 一. 列表数据的渲染
```
const shopArr = [
  {shopName: '大保健', amount: 110},
  {shopName: '大保健', amount: 110},
  {shopName: '大保健', amount: 110},
  {shopName: '大保健', amount: 110},
  {shopName: '大保健', amount: 110},
]

const shopTitle = [
  {title: '商店名称', dataIndex: 'shopName'},
  {title: '订单数', dataIndex: 'amount'
]

shopTitle.map((value) => {
   return <Row>{shopsTopTitile.map((colItem, colIndex) => {
         return (
             <Col span={4}>{colItem.title}
                {shopsTopArr.map((rowItem, rowIndex) => {
                   return <Row>
                        {rowItem[colItem.dataIndex]}
                   </Row>                
                    
                })}
             </Col>
          )
        })
     }
</Row>
})
```