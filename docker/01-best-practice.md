# Best Practice

使用 Docker 的最佳實踐

## Use Non-Root User to Start the Main Process in Container

Ask the ChatGPT this question

### Question

When we build a container image. Is there a suggestion that we should create a new user to start the main process, instead of using root user, right?

> 提問：當我們建立一個容器映像檔，是不是建議應該新建一個用戶來啟動容器中的主程序，而非使用 root 來啟動？

### Answer

Yes, it is generally recommended to use a non-root user to run the main process in a container, rather than running the process as the root user. This is considered a security best practice.

> 回答：是的，通常建議使用一個非 root 用戶來執行容器中的主程序，而不是以 root 用戶的身份執行。這被認為是一種安全的最佳做法。

Running the process as a non-root user can help to limit the potential damage if the process is compromised or if there is a security vulnerability in the application running inside the container. If the process is running as the root user, an attacker who gains access to the process may be able to take control of the entire container and potentially even the host system.

> 如果程序被滲透或者在容器內運行的應用程式存在安全漏洞，以非 root 用戶身份執行該程序可以限制潛在的損害。
>
> 如果程序以 root 用戶身份執行，獲得程序訪問權的駭客可能會控制整個容器，甚至可能控制主機系統。

In addition to using a non-root user for the main process, it is also a good practice to run the container with the minimum required privileges and to restrict access to sensitive resources, such as ports and file systems, as appropriate. This can help to reduce the attack surface and improve the security of the container.

> 除了使用非 root 用戶執行主程序之外，以最小的權限運行容器，並酌情限制對敏感資源（如端口和文件系統）的訪問，也是一種良好的做法。這可以幫助減少攻擊面，提高容器的安全性。

### Example

Create a group and a user, both of their names are octane

```dockerfile
# create user and group in Dockerfile

ARG WWWUSER=1000
ARG WWWGROUP=1000

# "-m" flag means create home directory for the new user
# "-s" flag specifies the user's login shell. If this flag is not used, the default shell specified in the /etc/default/useradd file is used
# "--no-log-init" flag disables the initialization of the user's login shell
# "--no-user-group" flag prevents the creation of a new group with the same name as the new user. By default, when you create a new user with the useradd command, a new group with the same name as the user is also created
RUN groupadd --force -g $WWWGROUP octane \
    && useradd -ms /bin/bash --no-log-init --no-user-group -g $WWWGROUP -u $WWWUSER octane
```
