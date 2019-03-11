# 前端文件下载

## Blob
```javascript
      this.content = content
      this.filename = filename
      const blob = new Blob(this.content])
      if (window.navigator.msSaveOrOpenBlob) {
        // 兼容IE10
        navigator.msSaveBlob(blob, this.filename)
      } else {
        //  chrome/firefox
        let aTag = document.createElement('a')
        aTag.download = this.filename
        aTag.href = URL.createObjectURL(blob)
        aTag.click()
        URL.revokeObjectURL(aTag.href)
      }
```


```javascript
var xhr = new XMLHttpRequest();
var formData = new FormData();
formData.append('snNumber', $("#snNumber").val());
formData.append('spec', $("#spec").val());
formData.append('startCreateDate', $("#startCreateDate").val());
formData.append('endCreateDate', $("#endCreateDate").val());
formData.append('startActiveDate', $("#startActiveDate").val());
formData.append('endActiveDate', $("#endActiveDate").val());
formData.append('supplier', $("#supplier").val());
formData.append('state', $("#cboDeviceStatus").val());
xhr.open('POST', vpms.ajaxUrl + vpms.manageUserUrl + "exportExcelDevices", true);
xhr.setRequestHeader("accessToken", userInfo.accessToken);
xhr.responseType = 'blob';
xhr.onload = function (e) {
  if (this.status == 200) {
    var blob = this.response;
    var filename = "设备导出{0}.xlsx".format(vpms.core.date.format("yyyyMMddhhmmss"));
    var a = document.createElement('a');
    blob.type = "application/excel";
    var url = createObjectURL(blob);
    a.href = url;
    a.download = filename;
    a.click();
    window.URL.revokeObjectURL(url);
  }
};
xhr.send(formData);
```