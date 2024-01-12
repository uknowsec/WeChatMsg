
### 实现离线解密展示聊天记录


### config.json

```
{
'pid': 3824,
'version': '3.9.8.15',
'account': '',
'mobile': '',
'name': '',
'mail': 'None',
'wxid': '',
'filePath': 'D:\\dist',
'key': ''
}
```


### 代码修改
修改
https://github.com/uknowsec/WeChatMsg/blob/master/app/ui/tool/pc_decrypt/pc_decrypt.py#L59
如果本地config.json存在则从本地获取到key等信息
```
    def set_info(self, result):
        file_path = "config.json"
        if os.path.isfile(file_path):
            with open(file_path, 'r') as file:
                config_str = file.read()
            config_str = config_str.replace("'", "\"")
            result[0] = json.loads(config_str)
            file.close()
        print(result)
        if result[0] == -1:
            QMessageBox.critical(self, "错误", "请登录微信")
        elif result[0] == -2:
            QMessageBox.critical(self, "错误", "微信版本不匹配\n请更新微信版本为:3.9.8.25")
        elif result[0] == -3:
            QMessageBox.critical(self, "错误", "WeChat WeChatWin.dll Not Found")
        elif result[0] == -10086:
            QMessageBox.critical(self, "错误", "未知错误，请收集错误信息")
        else:
            self.ready = True
            self.info = result[0]
            self.label_key.setText(self.info['key'])
            self.lineEdit.setText(self.info['wxid'])
            self.label_name.setText(self.info['name'])
            self.label_phone.setText(self.info['mobile'])
            self.label_pid.setText(str(self.info['pid']))
            self.label_version.setText(self.info['version'])
            self.lineEdit.setFocus()
            self.checkBox.setCheckable(True)
            self.checkBox.setChecked(True)
            self.get_wxidSignal.emit(self.info['wxid'])
            directory = os.path.join(path.wx_path(), self.info['wxid'])
            if os.path.exists(directory):
                self.label_db_dir.setText(directory)
                self.wx_dir = directory
                self.checkBox_2.setCheckable(True)
                self.checkBox_2.setChecked(True)
                self.ready = True
            if self.ready:
                self.label_ready.setText('已就绪')
            if self.wx_dir and os.path.exists(os.path.join(self.wx_dir)):
                self.label_ready.setText('已就绪')
        self.stopBusy()
```

### 编译

```
conda activate py3.10
pip install -r requirements.txt
pyinstaller -F main.py --add-data="app/resources/data/;app/resources/data/"
```
