![image](https://github.com/user-attachments/assets/b8398f63-9128-497c-b880-28c03cb5f6c7)

# airflow
- XSHELL操作<br>
  1.登入Airflow主機10.231.220.68(sakura/sakura#123)<br>
  2.最高權限 sudo su<br>
  3.密碼:sakura#123<br>
![image](https://github.com/user-attachments/assets/f6b509a7-6603-4c63-9650-1bb43c22e43d)

- 進入dags存放路徑+創建資料夾(mkdir)+拖拉dag檔案進指定路徑<br>
  1.cd /opt/airflow/dags<br>
  2.docker ps<br>
  3.mkdir crawler_bitdefender -->程式拖拉進路徑(本次使用DAG + .ENV)<br>
  > ![image](https://github.com/user-attachments/assets/bce913fb-b259-41ee-99bd-0c687d74f3cd)
  
- 進入容器使用Airflow最高權限安裝程式必要的API
  1.查看容器狀態 docker ps<br>
  > ![image](https://github.com/user-attachments/assets/db9cc888-0655-4629-a658-0d65dc7722ec)<br>
  
  # <進入容器>------>docker exec -it --user root <container-id> /bin/bash
  > docker exec -it --user root airflow-airflow-webserver-1 bash<br>
  > cd /opt/airflow/dags//crawler_bitdefender/<br>
  
  > apt-get update && apt-get install -y \
    wget \
    unzip \
    fonts-liberation \
    libvulkan1 \
    libxrandr2 \
    libatk1.0-0 \
    libxdamage1 \
    libxcomposite1 \
    libasound2 \
    libgtk-3-0 \
    libpangocairo-1.0-0 \
    libnss3 \
    libx11-xcb1 \
    xdg-utils && \
    apt-get clean
  
  > wget https://mirror.cs.uchicago.edu/google-chrome/pool/main/g/google-chrome-stable//google-chrome-stable_114.0.5735.90-1_amd64.deb && \
    dpkg -i google-chrome-stable_114.0.5735.90-1_amd64.deb || apt-get -f install -y
  
  > wget https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_linux64.zip && \
    unzip chromedriver_linux64.zip && \
    mv chromedriver /usr/local/bin/ && \
    chmod +x /usr/local/bin/chromedriver 

  > 確認完成安裝版本<br>
  > google-chrome --version<br>
  > chromedriver --version<br>
  > 離開容器 exit
  ![image](https://github.com/user-attachments/assets/fc7b34d6-111a-41ce-a2f4-c9175aa6fe33)
  
  > docker exec -it --user root airflow-airflow-triggerer-1 bash
  > 同上作法
  > ![image](https://github.com/user-attachments/assets/9c1e4d09-88b7-4cdc-b640-c178bd4438d6)

  > docker exec -it --user root airflow-airflow-scheduler-1 bash
  > docker exec -it --user root airflow-airflow-worker-1 bash

  2.個別進入容器內安裝必要API(本次使用爬蟲會需要多瀏覽器)
  > docker exec -it --user airflow airflow-airflow-webserver-1 bash<br>
  > cd /opt/airflow/dags//crawler_bitdefender/<br>
  > python -m pip install --upgrade pip<br>
  > python -m pip --version<br>
  
  3.被要求最高權限可以用這個方式略過python -m pip install + 要安裝的API
  > python -m pip install selenium psycopg2-binary python-dotenv<br>
  > ![image](https://github.com/user-attachments/assets/0dd1b676-d2bf-4132-81f5-24b93134f139)
  
  > python -m pip install chromedriver-autoinstaller
  > ![image](https://github.com/user-attachments/assets/683ad141-5c71-4545-829a-27f97b5990c0)
  
# 解決安裝Google Chrome
  ![image](https://github.com/user-attachments/assets/f039da53-bca6-4323-8bbe-e6434d643ea4)
> wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb<br>
> apt-get update && apt-get install -y ./google-chrome-stable_current_amd64.deb<br>
> google-chrome --version<br>
  - 容器外執行(下載+安裝)
  > ![image](https://github.com/user-attachments/assets/b91b400c-371d-4335-a38f-453b3eb4b8bb)
  > ![image](https://github.com/user-attachments/assets/ec3b1281-3a57-462b-be9d-e2e6f85ee515)
  - 完成畫面
  > ![image](https://github.com/user-attachments/assets/5ad336ef-21a4-4442-89c1-7b3eb6a01b1b)

  4.先跳出容器...再回到1把全部容器安裝必要API
  > exit<br>
  > docker exec -it --user airflow airflow-airflow-triggerer-1 bash<br>
  > docker exec -it --user airflow airflow-airflow-scheduler-1 bash<br>
  > exit<br>
  > docker exec -it --user airflow airflow-airflow-worker-1 bash<br>
  > exit<br>
  4-2爬蟲需要有瀏覽器
  > python -m pip install google-chrome-stable
  > 完成後記得要重啟

  5.重啟容器
  docker restart airflow-airflow-webserver-1<br>
  docker restart airflow-airflow-triggerer-1<br>
  docker restart airflow-airflow-scheduler-1<br>
  docker restart airflow-airflow-worker-1<br>
  docker restart airflow-postgres-1<br>
  docker restart airflow-redis-1<br>

  

