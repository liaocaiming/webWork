```
interface Iprops {
  typeName: string
}

class Storage {
  private typeName: string;

  constructor (props:Iprops) {
    this.init(props)
    this.typeName = props.typeName
  }

  public init (props:Iprops) {
    this.bind()
    this.trigger(props.typeName)
  }

  public trigger (typeName:string) {
    // 新选项卡如果没有sessionStorage, 触发上个页面localStorage.setItem
    if(!sessionStorage.getItem(typeName)){
      localStorage.setItem("getSession", String(Date.now()));
    }
    return
  }

  public bind () {
    window.addEventListener("storage", this.listener)
  }

  public listener = (event:any) => {
    if (!event.newValue) {
      return ;
    }
    
    // 上个页面监听新选项卡localStorage.setItem
    if(event.key === "getSession"){
      localStorage.setItem("storeSessionData", sessionStorage.getItem(this.typeName) || '');
      localStorage.removeItem("storeSessionData");
    }
    if(event.key === "storeSessionData"){
      sessionStorage.setItem(this.typeName, event.newValue);
      localStorage.removeItem("getSession");
    }
    if(event.key === "updateSession"){
      sessionStorage.setItem(this.typeName, event.newValue);
      localStorage.removeItem("updateSession");
    }
  }

  public update (data:any) {
    localStorage.setItem('updateSession', JSON.stringify(data));
  }

  public unbind () {
    window.removeEventListener('storage', this.listener)
  }
}

export default Storage;
```