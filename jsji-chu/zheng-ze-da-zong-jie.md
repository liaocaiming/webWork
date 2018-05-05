###### 手机号码
```
/^(13|14|15|17|18|19)\d{9}$/
```
```
const getpattern = {
  mobile: /^1\d{10}$/,
  areaCode: /^\d+$/,
  fixedPhone: /^\d+$/,
  alphabetOrNumber: /^[A-Za-z0-9]+$/,
  characterOrNumber: /^[A-Za-z0-9\u4e00-\u9fa5]+$/,
  userName: /^[A-Za-z0-9\u4e00-\u9fa5]*[A-Za-z0-9]*$/,
  floatNumber: /^\d+(\.\d+)?$/,
  //正数
  positiveNumber: /^([1-9]+(\.\d+)?)|(0\.\d+)$/,
  // 两位小数的正则
  TwoDecimalsNumber: /^([1-9]\d*(\.\d{1,2})?|0\.[1-9][0-9]?|0\.[0-9][1-9])$/,

  // 会员等级名称
  memberLevelName: /^([\u4e00-\u9fa5]{1,8}|[A-Za-z]{1,20})$/,
  // 整数;
  number: /^0|([1-9]\d*)$/,
  // 正整数
  positiveInteger: /^[1-9]\d*$/,
  // 1到90的正整数（天数限制）
  oneToNinety: /^[1-8][0-9]?$|^90$|^9$/,
  // 1 到 9999, 最大积分限制
  maxPoints: /^[1-9]\d{0,3}$/,
  // 消费的最大金额
  expenseMaxMoney: /^[1-9]\d{0,5}$/,
  // 固定电话正则:
  fixedNumber: /^\d{3,4}\-\d+(\-\d+)?$/,
  // 会员卡升降级条件
  memberMaxPrice: /^[1-9]\d{0,7}$/,
  // 限制营业额指标 0-100000000;
  limitAmountTarget: /^[1-9]\d{0,8}$|^100000000$|^0$/
}

// 过滤没有数据的搜索参数
export function validateSearChValue(SearchObj) {
  if (!SearchObj) return;
  let obj: any;
  Object.keys(SearchObj).forEach((key) => {
    if (SearchObj[key] !== '' && SearchObj[key] !== undefined && SearchObj[key] !== null) {
      obj[key] = SearchObj[key]
    }
  })
  return obj;
}
```


