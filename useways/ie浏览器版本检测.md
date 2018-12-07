# ie浏览器版本检测

```javascript
fnIeVer();
function fnIeVer(){
    var oldIEDetect = false;
    var oldIENotSupport = false;

    if(/MSIE (\d+\.\d+);/.test(navigator.userAgent)){
        var ieversion = new Number(RegExp.$1);
        if (ieversion == 8) {
            oldIEDetect = true;
        } else if (ieversion < 8) {
            alert("此页面支援Internet Explorer 8或以上之版本，建议您使用Chrome等浏览器以获得最佳体验。\n\nThis website supports Internet Explorer 8 and above versions only, please upgrade your browser to get the best possible experience.");
            oldIENotSupport = true;
        }
    }
}
```