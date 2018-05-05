```
 if (this.downNum < 60 || this.isClick) {
      return;
    }
    this.isClick = true;
    user.fetchMobileCode({
      mobile: this.mobile
    }).then(res => {
      if (res.code == 400004) {
        this.downNum = 60;
        this.isClick = false;
        this.setData({
          mobile_err: res.msg,
          codeMsg: '重新发送'
        })
        clearInterval(this.timer);
        return;
      }
    })

    this.timer = setInterval(() => {
      this.downNum--;
      if (this.downNum <= 0) {
        this.downNum = 60;
        this.isClick = false;
        this.setData({
          codeMsg: '重新发送'
        })
        clearInterval(this.timer);
        return
      }
      this.setData({
        codeMsg: `${this.downNum} 秒`,

      })
    }, 1000)
```