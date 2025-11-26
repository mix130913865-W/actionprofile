# ----------------------------------------------------
# 使用 Maven 官方映像作為建置階段 (BUILD_IMAGE)
# 版本：Maven 3.9.9 + Eclipse Temurin JDK 21 + Ubuntu Jammy
# ----------------------------------------------------
FROM maven:3.9.9-eclipse-temurin-21-jammy AS BUILD_IMAGE

# 將當前目錄的專案檔案複製到容器的 /app 目錄
COPY . /app

# 設定工作目錄為 /app
WORKDIR /app

# 使用 Maven 安裝與建置專案
# 會執行：下載依賴、編譯、測試、打包
RUN mvn install

# ----------------------------------------------------
# 使用官方 Tomcat 映像作為運行階段
# 版本：Tomcat 10 + JDK 21
# ----------------------------------------------------
FROM tomcat:10-jdk21

# 刪除 Tomcat 預設的 webapps 內容，避免 ROOT.war 與預設應用衝突
RUN rm -rf /usr/local/tomcat/webapps/*

# 從前一個建置階段複製打包好的 WAR 檔案到 Tomcat webapps 目錄
# 並命名為 ROOT.war，讓應用直接以根路徑運行
COPY --from=BUILD_IMAGE /app/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# 開放 8080 端口供外部存取
EXPOSE 8080

# 容器啟動時執行 Tomcat 服務
CMD ["catalina.sh", "run"]
