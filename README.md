運作架構圖[draw.io](https://app.diagrams.net/#G1SZLu9U9IzWNq1JYq6qyq4-XeGFb3CpVN#%7B%22pageId%22%3A%22IKRYCP0CiP5juJX1pNyO%22%7D)
![image](https://github.com/user-attachments/assets/09c4b08b-6683-4bb9-97d2-c337b3897942)

# Airflow
- [測試區](http://10.231.220.68:8080/home)
- [正式區](http://10.231.250.68:8080/home)

- XSHELL操作<br>
  1.登入Airflow主機10.231.220.68(ness/ness)<br>
  2.最高權限 sudo su<br> 
  3.密碼:ness<br>
![image](https://github.com/user-attachments/assets/f6b509a7-6603-4c63-9650-1bb43c22e43d)<br>
- 查看容器狀態<br>
  > docker ps<br>
  > ![image](https://github.com/user-attachments/assets/db9cc888-0655-4629-a658-0d65dc7722ec)<br>
  
- 進入dags存放路徑+創建資料夾(mkdir)+拖拉dag檔案進指定路徑<br>
  > cd /opt/airflow/dags<br>
  > docker ps<br>
  > mkdir crawler_bitdefender -->將程式拖拉進路徑(本次使用DAG + .ENV)<br>
  > ![image](https://github.com/user-attachments/assets/bce913fb-b259-41ee-99bd-0c687d74f3cd)

# 1.root權限安裝進入容器(需要各別安裝套件)<br>
> docker exec -it --user root airflow-airflow-webserver-1 bash<br>
docker exec -it --user root airflow-airflow-triggerer-1 bash<br>
docker exec -it --user root airflow-airflow-scheduler-1 bash<br>
docker exec -it --user root airflow-airflow-worker-1 bash

# 2.進入專案存放路徑<br>
> cd /opt/airflow/dags/crawler_bitdefender

# 3.pip install 安裝套件(過程可能會有失效的工具可以刪除)<br>
  > pip3 install selenium<br>
    pip3 install psycopg2-binary<br>
    pip3 install python-dotenv<br>
  檢查<br>
  python3 -c "import selenium; print(selenium.__version__)"<br>
  python3 -c "import psycopg2; print(psycopg2.__version__)"<br>
  python3 -c "from dotenv import load_dotenv; print('dotenv is working')"<br>

# 4.爬蟲必要安裝(選用版本號: 114.0.5735.90)<br>
[來源google-chrome](https://mirror.cs.uchicago.edu/google-chrome/pool/main/g/google-chrome-stable/)<br>
[來源chromedriver](https://chromedriver.storage.googleapis.com/)<br>
作法:個別進入容器內下<br>
4-1.安裝套件<br>
    apt-get update && apt-get install -y \     wget \    unzip \    fonts-liberation \    libvulkan1 \    libxrandr2 \    libatk1.0-0 \    libxdamage1 \    libxcomposite1 \    libasound2 \    libgtk-3-0 \
    libpangocairo-1.0-0 \    libnss3 \    libx11-xcb1 \    xdg-utils && \    apt-get clean

4-2.安裝google-chrome<br>
wget https://mirror.cs.uchicago.edu/google-chrome/pool/main/g/google-chrome-stable/google-chrome-stable_114.0.5735.90-1_amd64.deb && \
dpkg -i google-chrome-stable_114.0.5735.90-1_amd64.deb || apt-get -f install -y

4-3.安裝chromedriver<br>
wget https://chromedriver.storage.googleapis.com/114.0.5735.90/chromedriver_linux64.zip && \
unzip chromedriver_linux64.zip && \
mv chromedriver /usr/local/bin/ && \
chmod +x /usr/local/bin/chromedriver 

4-4.檢查安裝版本一致<br>
google-chrome --version && chromedriver --version<br>
  
4-5.離開容器<br>
> exit<br>
PS.所有容器皆需要安裝

# 5.重啟容器<br>
> docker restart airflow-airflow-webserver-1<br>
  docker restart airflow-airflow-triggerer-1<br>
  docker restart airflow-airflow-scheduler-1<br>
  docker restart airflow-airflow-worker-1<br>
  docker restart airflow-postgres-1<br>
  docker restart airflow-redis-1<br>

  

