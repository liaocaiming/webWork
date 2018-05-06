#### 1.数字动态增加

```
dynamicNumber() {
    let { duration = 2, goalNum, currency } = this.props;
    if (!goalNum) {
        this.setState({ num: goalNum || 0 })
        return;
    }
    if (typeof goalNum === 'string') {
        goalNum = parseFloat(goalNum).toFixed(2);
        return;
    }
    if (typeof this.state.num === 'string') {
        return;
    }
    if (this.state.num >= Math.abs(goalNum)) {
        if (this.props.currency) {
            this.setState({ num: `${helpers.midifyCurrencyNoSymbol(goalNum, currency)}` });
            window.cancelAnimationFrame(this.stopRequestAnimationFrame);
            return;
        }
        this.setState({ num: `${helpers.midifyCurrencyNoSymbol(goalNum, currency, 0)}` });
        return;
    }
    this.setState({ num: this.state.num + Math.ceil(goalNum / 60 / duration) });
    this.stopRequestAnimationFrame = requestAnimationFrame(this.dynamicNumber);
}
```