# chrome

启用Flash调试

==============================

1、chrome://plugins/

2、点击详细信息

3、禁用chrome自带flash

4、安装flash debug版本 https://www.adobe.com/support/flashplayer/debug_downloads.html

 

【下载crx】

http://chrome-extension-downloader.com/

kieadkdifhfdpooogdnanljolkekceij

用插件地址替换###

https://clients2.google.com/service/update2/crx?response=redirect&x=id%3D###%26uc

【查看源代码】

将后缀名crx修改为rar，解压

【Chrome数据持久化】

内存持久：

chrome.extension.getBackgroundPage();

bgPage[tab.url] = data;

本地持久：

保存在本地存储，localStorage[tab] = data;

同步持久：

将数据上传至服务器
