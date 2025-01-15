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
  
- 進入容器使用Airflow最高權限安裝程式必要的API
  1.查看容器狀態 docker ps
  > ![image](https://github.com/user-attachments/assets/db9cc888-0655-4629-a658-0d65dc7722ec)
  > docker exec -it --user airflow airflow-airflow-webserver-1 bash
  > docker exec -it --user airflow airflow-airflow-triggerer-1 bash
  > docker exec -it --user airflow airflow-airflow-scheduler-1 bash
  > docker exec -it --user airflow airflow-airflow-worker-1 bash

  2.個別進入容器內安裝必要API(本次使用爬蟲會需要多瀏覽器)
  > docker exec -it --user airflow airflow-airflow-webserver-1 bash
  > cd /opt/airflow/dags
  > python -m pip install --upgrade pip
  > python -m pip --version
  
  3.被要求最高權限可以用這個方式略過python -m pip install + 要安裝的API
  > python -m pip install selenium psycopg2-binary python-dotenv
  > ![image](https://github.com/user-attachments/assets/0dd1b676-d2bf-4132-81f5-24b93134f139)

  4.先跳出容器...再回到1把全部容器安裝必要API
  > exit
  > docker exec -it --user airflow airflow-airflow-triggerer-1 bash
  > docker exec -it --user airflow airflow-airflow-scheduler-1 bash
  > exit
  > docker exec -it --user airflow airflow-airflow-worker-1 bash
  > exit

  5.重啟容器
  docker restart airflow-airflow-webserver-1
  docker restart airflow-airflow-triggerer-1
  docker restart airflow-airflow-scheduler-1
  docker restart airflow-airflow-worker-1
  docker restart airflow-postgres-1
  docker restart airflow-redis-1

  

