
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
修改WeChatMsg/blob/master/app/ui/tool/pc_decrypt/pc_decrypt.py#L277C10-L277C10
如果本地config.json存在则从本地获取到key等信息
```
    def run(self):
        file_path = "config.json"
        if os.path.isfile(file_path):
            with open(file_path, 'r') as file:
            config_str = file.read()
            config_str = config_str.replace("'", "\"")
            result = json.loads(config_str)
            file.close()
        else:
            file_path = './app/resources/data/version_list.json'
            if not os.path.exists(file_path):
                resource_dir = getattr(sys, '_MEIPASS', os.path.abspath(os.path.dirname(__file__)))
                file_path = os.path.join(resource_dir, 'app', 'resources', 'data','version_list.json')
            with open(file_path, "r", encoding="utf-8") as f:
                VERSION_LIST = json.loads(f.read())
            try:
                result = get_wx_info.get_info(VERSION_LIST)
                if result == -1:
                    result = [result]
                elif result == -2:
                    result = [result]
                elif result == -3:
                    result = [result]
            except:
                logger.error(traceback.format_exc())
                result = [-10086]
        self.signal.emit(result)

```
