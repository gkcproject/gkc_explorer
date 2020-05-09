# gkc_explorer
準備事項  
1教程部署環境為ubuntu18.04 理論上兼容16.04  
2同步到最新的gkcd的數據  
3 服務器配置推薦4核 8G以上的配置  


#部署塊瀏覽器前端  
安裝nginx  
sudo apt-get install nginx  

修改nginx配置  /etc/nginx/sites-available/default   
server {  
        listen 80;  
        listen [::]:80;  
        server_name explorer.gkc.cash; //域名  
        root /home/explorer_www;       //文件目錄  
        index index.html;  
        location / {  
        try_files $uri $uri/ /index.html;  
        }  
        location /api/{  
        proxy_pass http://127.0.0.1:2028/api/;  
        }  
        }  

重啟nginx  
/etc/init.d/nginx restart  

#部署塊瀏覽器後端  
安裝mongo db  
sudo apt-get install mongodb  

#限制mongo db內存使用大小 （針對內存較小的機器，大內存可忽略）  
修改 vi /etc/mongodb.conf  
添加 wiredTigerCacheSizeGB = 2  
重啟mongodb  
/etc/init.d/mongodb restart  

修改gkc.conf配置增加下面幾行  
rpcbind=127.0.0.1  
rpcallowip=127.0.0.1/255.255.255.255  
rpcuser=username  
rpcpassword=123456  

上傳explorer_main到服務器  
#修改conf/main.toml 默認已經配置好對應的參數  


啟動後端程序  
nohup ./explorer_main &  
查看相關信息  
tail –f nohup  
