## Ionic Framework
官方地址：[http://ionicframework.com](http://ionicframework.com/)

这个一个根据ionic官方的组件库改良的组件库，为了迎合不同的项目需要而做针对官方的组件库进行一些修改。修改内容为：DateTime、Picker、Popover

### DateTime

官方DateTime组件返回的时间格式是 ISO，这不是我们想要的时间格式，我们通过官方推荐的moment.js，引入到依赖包路径为：/node_modules/ionic-angular/components/datetime/datetime.js的datetime.js文件通过利用momentjs的转换，把ISO的时间格式转换成符合中国时区的时间格式
```
 // var nowString = (new Date).toISOString();
 var nowString = moments((new Date).toISOString()).utcOffset(8).format();

/**
    * 这里的转换主要针对ionChange事件的数据返回
    * @hidden
    */
  DateTime.prototype._inputChangeEvent = function () {
      return moments(this.value).utcOffset(8).format();
  };
  /**
    * @这里主要针对时间控件中的确认事件所返回的数据
    */
  DateTime.prototype._inputNgModelEvent = function () {
      let date = convertDataToISO(this.value);
      return moments().format(date);
  };
```
### Picker

picker组件主要用于时间控件datetime，当时间格式为：YYYY-MM-DD HH:mm 的时候，时间控件就会显示的非常密集，难以阅读，于是乎我们把时间控件改成带单位的时间，例如：2018年6月20日10时20分。

修改路径：node_modules/ionic-angular/components/picker/picker-component.js

```
if (isPresent(inputOpt)) {
    if (isString(inputOpt) || isNumber(inputOpt)) {
        opt.text = inputOpt.toString();
        opt.value = inputOpt;
    } else {
        if(column.name === 'year'){
            opt.text =  isPresent(inputOpt.text) ? inputOpt.text + '年' : inputOpt.value+ '年';
        }else if(column.name === 'month'){
            opt.text = isPresent(inputOpt.text) ? inputOpt.text + '月' : inputOpt.value+ '月';
        }else if(column.name === 'day'){
            opt.text =  isPresent(inputOpt.text) ? inputOpt.text + '日' : inputOpt.value+ '日';
        }else if(column.name === 'minute'){
            opt.text =  isPresent(inputOpt.text) ? inputOpt.text + '时' : inputOpt.value+ '时';
        }else if(column.name === 'hour'){
            opt.text =  isPresent(inputOpt.text) ? inputOpt.text + '时' : inputOpt.value+ '时';
        }else if(column.name === 'minute'){
            opt.text = isPresent(inputOpt.text) ? inputOpt.text + '分' : inputOpt.value+ '分';
        }else{
            opt.text = isPresent(inputOpt.text) ? inputOpt.text : inputOpt.value;
        }
        // opt.text = isPresent(inputOpt.text) ? inputOpt.text : inputOpt.value;
        opt.value = isPresent(inputOpt.value) ? inputOpt.value : inputOpt.text;
    }
}
```


### Popover

Popover是弹出窗口是显示在当前页面顶部的对话框。它可以用于任何事情，但通常用于不适合导航栏的溢出操作。主要是ios平台和Android平台出现的动画效果和页面效果都不一样，在项目中，更多希望Android平台的能统一跟ios平台一样。

修改路径：node_modules/ionic-angular/components/popover/popover-transitions.js、/node_modules/ionic-angular/components/popover/popover.md.scss

原来的
```
 PopoverMdPopIn.prototype.play = function () {
    var _this = this;
    this.plt.raf(function () {
        _this.mdPositionView(_this.enteringView.pageRef().nativeElement, _this.opts.ev);
        _super.prototype.play.call(_this);
    });
};
```
改为
```
 PopoverMdPopIn.prototype.play = function () {
    var _this = this;
    this.plt.raf(function () {
        _this.iosPositionView(_this.enteringView.pageRef().nativeElement, _this.opts.ev);
        _super.prototype.play.call(_this);
    });
};
```