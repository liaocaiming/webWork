#### 1、判断一个字符串中出现次数最多的字符，统计这个次数

```
 function getMaxString (str) {
    var json = {};
    for(var i =0; i < str.length -1 ; i++) {
        if (!json[i]) {
            json[i] = 1;
        } else {
            json[i]++;
        }
    }
    
    var max = 0;
    var s = '';
    for (var i in json) {
        if (json[i] > max) {
            max = json[i];
            s = i
        }
    }
    return max
 }
```

#### 2、编写一个方法 去掉一个数组的重复元素

###### 方法一.
```
var arr = [0,2,3,4,4,0,2];
var obj = {};
var tmp = [];
for(var i = 0 ;i< arr.length;i++){
   if( !obj[arr[i]] ){
      obj[arr[i]] = 1;
      tmp.push(arr[i]);
   }
}
console.log(tmp);
```

##### 方法二.
```
var arr = [2,3,4,4,5,2,3,6];
var arr2 = arr.filter(function(element,index,self){
    return self.indexOf(element) === index;
});
console.log(arr2);
```

#### 2. 将一维数组转化成二维数组
```
dyadicArray(data, num) {
    let arr = [];
    while (data.length > 0) {
      arr.push(data.splice(0, num));
    }
    return arr;
},
```

#### 3. 计算选中的最里层数据的个数, 用来做 '至少选择一项'  做判断;
```
 calCulSelectAllCheckboxNumber(arr) {
    let num = 0;
    if (!arr) {
        return false;
    }
    function fn(arr) {
        arr.forEach(value => {
            if (value.child && value.child.length) {
                fn(value.child);
            } else {
                if (value.checked) {
                    num++
                }
            }
        })
    }
    fn(arr);
    return num;
}
```

#### 4. 将数字修饰成带货币符号的货币字符串

```
midifyCurrency(number, symbol = '￥', places = 2, thousand = ',', decimal = '.') {
  if (number === '-') return '-'
  number = number || 0;
  places = !isNaN(places = Math.abs(places)) ? places : 2;
  symbol = symbol !== undefined ? symbol : "$";
  thousand = thousand || ",";
  decimal = decimal || ".";
  var negative = number < 0 ? "-" : "",
    i: any = parseInt(number = Math.abs(+number || 0).toFixed(places), 10) + "",
    j = (j = i.length) > 3 ? j % 3 : 0;
  return symbol + negative + (j ? i.substr(0, j) + thousand : "") + i.substr(j).replace(/(\d{3})(?=\d)/g, "$1" + thousand) + (places ? decimal + Math.abs(number - i).toFixed(places).slice(2) : "");
}
```

#### 5.对微信小程序请求接口数据的封装

```
const fetch = (method, url, params) => {
  const app = getApp();
  const data = app.globalData;
  return new Promise((resolve, reject) => {
    console.log(66, method, data.extConfig.apiUrl + url, params) // 应后台要求测试环境打印数据
    let allUrl;
    if (!data.token) {
      allUrl = `${data.extConfig.apiUrl}${url}`;
    } else {
      allUrl = `${data.extConfig.apiUrl}${url}?token=${data.token}`;
    }
    wx.showLoading({
      title: '加载中',
      mask: true
    })
    wx.request({
      url: allUrl,
      data: params,
      method: method,
      success(res) {
        console.log(res, url);
        wx.hideLoading();
        if (res.statusCode === 401) {
          let tmp = getCurrentPages()
          let tmpArr = tmp[tmp.length - 1].__route__.split('/')
          let tmpStr = tmpArr[tmpArr.length - 1]
          wx.removeStorageSync('token');
          wx.reLaunch({ url: tmpStr });
          return
        }
        if (res.statusCode != 200 && res.statusCode != 304) {
          wx.showToast({
            title: res.data.message,
            image: '/static/icons/err.png',
            duration: 2000
          })
          reject(res.data);
          return;
        }
        resolve(res.data)
      },
      fail(res) {
        wx.hideLoading();
        if (res.statusCode != 200 && res.data) {
          wx.showToast({
            title: res.data.message,
            image: '/static/icons/err.png',
            duration: 2000
          })
        }
        reject(res.data)
      },
      complete () {
        wx.hideLoading();
      }
    })
  })
}

```

#### 6.微信小程序登入逻辑的封装
```
/ 对login的二次分装
    ready() {
        let that = this
        return new Promise((resolve, reject) => {
            let token = wx.getStorageSync('token');
            if (!token) {
                that.login().then((res) => {
                    resolve(res)
                }).catch((res) => {
                    reject(res)
                })
            } else {
                that.globalData.token = token;
                resolve()
            }
        })
    },

    // 授权登录接口处理处理
    login() {
        // wx.showLoading({
        //     title: '登录中',
        //     mask: true
        // })
        let that = this
        return new Promise((resolve, reject) => {
            wx.login({
                success: (res) => {
                    // wx.hideLoading();
                    if (res.code) {
                        wx.getUserInfo({
                            success: (data) => {
                                wx.hideLoading();
                                let userInfo = data.userInfo;
                                user.fetchLoginInfo({
                                    code: res.code,
                                    avatar: userInfo.avatarUrl,
                                    nickname: userInfo.nickName,
                                    sex: userInfo.gender,
                                    mall_id: that.globalData.mall_id
                                }).then(res => {
                                    console.log(res, 'res => token');
                                    let token = res.token;
                                    that.globalData.token = token;
                                    wx.setStorageSync('token', token);
                                    wx.setStorageSync('loginInfo', res);
                                    resolve(res);
                                }).catch(err => {
                                    reject();
                                })
                            },
                            fail(res) { // 用户拒绝授权就跳到异常页面
                                reject()
                                wx.hideLoading();
                                // let temp = getCurrentPages()
                                // let tempNum = temp[temp.length - 1].__route__.match(/\//).length
                                // let tempStr = '../'.repeat(tempNum)
                                wx.removeStorageSync('token')
                                wx.removeStorageSync('loginInfo');
                                // wx.reLaunch({
                                //     url: tempStr + "forbidden/index"
                                // })
                                wx.openSetting({
                                    success: (res) => {
                                      /*
                                       * res.authSetting = {
                                       *   "scope.userInfo": true,
                                       *   "scope.userLocation": true
                                       * }
                                       */
                                    }
                                  })
                            }
                        })
                    } else {
                        wx.hideLoading();
                        reject(res);
                    }
                },
                fail: (err) => {
                    reject()
                }
            })
        })
    },
```

#### 7. 处理浏览器计算精度的方法(加, 减, 乘, 除);
```
export let add = (arg1, arg2) => {
  let r1
  let r2
  let m
  let c
  try {
    r1 = arg1.toString().split('.')[1].length
  } catch (e) {
    r1 = 0
  }
  try {
    r2 = arg2.toString().split('.')[1].length
  } catch (e) {
    r2 = 0
  }
  c = Math.abs(r1 - r2)
  m = Math.pow(10, Math.max(r1, r2))
  if (c > 0) {
    let cm = Math.pow(10, c)
    if (r1 > r2) {
      arg1 = Number(arg1.toString().replace('.', ''))
      arg2 = Number(arg2.toString().replace('.', '')) * cm
    } else {
      arg1 = Number(arg1.toString().replace('.', '')) * cm
      arg2 = Number(arg2.toString().replace('.', ''))
    }
  } else {
    arg1 = Number(arg1.toString().replace('.', ''))
    arg2 = Number(arg2.toString().replace('.', ''))
  }
  return (arg1 + arg2) / m
}

export let sub = (arg1, arg2) => {
  let r1
  let r2
  let m
  let n
  try {
    r1 = arg1.toString().split('.')[1].length
  } catch (e) {
    r1 = 0
  }
  try {
    r2 = arg2.toString().split('.')[1].length
  } catch (e) {
    r2 = 0
  }
  m = Math.pow(10, Math.max(r1, r2))
  n = (r1 >= r2) ? r1 : r2
  return Number(((arg1 * m - arg2 * m) / m).toFixed(n))
}

export let mul = (arg1, arg2) => {
  let m = 0
  let s1 = arg1.toString()
  let s2 = arg2.toString()
  try {
    m += s1.split('.')[1].length
  } catch (e) { }
  try {
    m += s2.split('.')[1].length
  } catch (e) { }
  return Number(s1.replace('.', '')) * Number(s2.replace('.', '')) / Math.pow(10, m)
}

export let div = (arg1, arg2) => {
  let t1 = 0
  let t2 = 0
  let r1
  let r2
  try {
    t1 = arg1.toString().split('.')[1].length
  } catch (e) { }
  try {
    t2 = arg2.toString().split('.')[1].length
  } catch (e) { }
  r1 = Number(arg1.toString().replace('.', ''))
  r2 = Number(arg2.toString().replace('.', ''))
  return (r1 / r2) * Math.pow(10, t2 - t1)
}
```

####     8、多维数组转一位数组：原题：[1,[2,3]] => [1,2,3]；
```
        var arr = [1,[2,3]];
        function fun(arr){
        	if (Object.prototype.toString.call(arr) != '[object Array]') {
        		console.log(111)
        		return;
        	};
        	var newArr = [];
        	function fn(arr){
        		for (var i = 0; i < arr.length; i++) {
        			if (arr[i].length) {
        				//递归调用
        				fn(arr[i])
        			}else {
                        newArr.push(arr[i])
        			}
        		};
        	}
        	fn(arr);
        	return newArr;
        }
        arr = fun(arr)
```

#### 9. toFixed的坑
```
 0.005.toFixed(2) // "0.01"
 1.005.toFixed(2) // "1.00"
```
  解决方法
```
 let round = (value, number) => {
  let num = number === undefined ? 2: parseInt(number);
  let n = Math.round(mul(parseFloat(value), Math.pow(10, num)));
  let val = div(n, Math.pow(10, num));
  return val;
}
```






