# Use Windows Server Core as the base image
FROM mcr.microsoft.com/windows/servercore:ltsc2019

# Install IIS using DISM
RUN dism.exe /online /enable-feature /all /featurename:iis-webserver /NoRestart

# Set up work directory
WORKDIR C:/java

# Copy installation files
ADD ./setup C:/java/setup

# Install JRE and JDK
RUN powershell -Command "Start-Process -FilePath 'C:\\java\\setup\\jre.cmd' -NoNewWindow -Wait; \
     Start-Process -FilePath 'C:\\java\\setup\\jdk.cmd' -NoNewWindow -Wait"

# Set environment variables for Java
ENV JAVA_HOME="C:\\Program Files\\Java\\jdk1.8.0_202"
ENV JRE_HOME="C:\\Program Files\\Java\\jre1.8.0_202"
ENV PATH="%JAVA_HOME%\\bin;%JRE_HOME%\\bin;%PATH%"


# Copy Tomcat files directly with Docker COPY command
COPY ./setup/tomcat C:/tomcat

# Alternative method using cmd array syntax for Docker RUN
RUN ["cmd", "/c", "C:/java/setup/tomcatservice.bat"]

# Use the full path to sc.exe
RUN C:/Windows/System32/sc.exe config tomcat9 obj=LocalSystem start=auto


# Expose Tomcat port
EXPOSE 8080

# Start Command Prompt and keep container running
CMD ["cmd", "/k"]
