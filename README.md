![image](https://github.com/user-attachments/assets/b8398f63-9128-497c-b880-28c03cb5f6c7)# airflow
- XSHELL操作<br>
  1.登入Airflow主機10.231.220.68(sakura/sakura#123)<br>
  2.最高權限 sudo su<br>
  3.密碼:sakura#123<br>
![image](https://github.com/user-attachments/assets/f6b509a7-6603-4c63-9650-1bb43c22e43d)

- 進入dags存放路徑+創建資料夾(mkdir)+拖拉dag檔案進指定路徑
  1.cd /opt/airflow/dags
  2.docker ps
  3.mkdir crawler_bitdefender -->程式拖拉進路徑(本次使用DAG + .ENV)
  > ![image](https://github.com/user-attachments/assets/bce913fb-b259-41ee-99bd-0c687d74f3cd)
  
- 進入容器使用airflow最高權限
  1.查看容器狀態 docker ps
  > ![image](https://github.com/user-attachments/assets/db9cc888-0655-4629-a658-0d65dc7722ec)
 
  

