# Volume 介紹

在 Docker 中，**Volume（卷）** 是用來持久化數據的機制，允許容器和宿主機之間共享數據，並保證容器銷毀後數據依然存在。

---

### **Volume 的主要功能**

1. **數據持久化**
    - 容器的文件系統是臨時的，當容器刪除後其內部數據也會消失。
    - Volume 則存儲數據在宿主機上，容器即使刪除，數據仍然保留。
2. **數據共享**
    - 不同的容器可以通過同一個 Volume 共享數據，方便多容器協作。
    - 例如，一個容器負責生產數據，另一個容器負責消費數據。
3. **數據分離**
    - Volume 把數據存儲和應用的執行分離，這樣可以讓數據的管理更加靈活。
4. **性能優化**
    - 與直接掛載宿主機目錄相比，Volume 提供更好的性能和隔離。

---

### **Volume 的種類**

1. **匿名 Volume**
    - Docker 自動分配一個沒有名字的 Volume。
    - 用於臨時數據存儲，且通常在容器刪除時一併刪除。
    - 範例：
        
        ```bash
        docker run -v /app/data busybox
        
        ```
        
2. **命名 Volume**
    - 自己明確指定 Volume 的名字。
    - 範例：
        
        ```bash
        docker volume create my_data
        docker run -v my_data:/app/data busybox
        
        ```
        
3. **Bind Mount（綁定掛載）**
    - 把宿主機的某個目錄直接掛載到容器內。
    - 與 Volume 不同，Bind Mount 直接使用宿主機的路徑，沒有 Docker 管理。
    - 範例：
        
        ```bash
        docker run -v /host/path:/container/path busybox
        
        ```
        

---

### **Volume 的優勢**

- **易管理**: Docker 提供專用命令來管理 Volume，如 `docker volume ls`、`docker volume inspect` 等。
- **容器獨立**: 與容器的生命週期分離，容器刪除後數據依然存在。
- **安全隔離**: Volume 存儲在 Docker 管理的專用目錄中，比 Bind Mount 更安全。

---

### **Volume 的常見用途**

1. **持久化應用數據**
    
    如資料庫（MySQL、PostgreSQL）的數據存儲目錄。
    
2. **共享配置文件**
    
    多個容器可以通過 Volume 共享相同的配置文件或憑證。
    
3. **開發環境同步**
    
    開發者可以掛載宿主機的代碼目錄到容器內，實現快速測試和部署。
    

---

### **Volume 的命令**

1. **創建 Volume**
    
    ```bash
    docker volume create my_volume
    
    ```
    
2. **列出 Volume**
    
    ```bash
    docker volume ls
    
    ```
    
3. **檢查 Volume**
    
    ```bash
    docker volume inspect my_volume
    
    ```
    
4. **刪除 Volume**
    
    ```bash
    docker volume rm my_volume
    
    ```
    

---

### 範例：使用 Volume

### 創建一個帶 Volume 的容器

```bash
docker run -d -v my_volume:/app/data --name my_container busybox

```

### 查看 Volume

```bash
docker volume ls

```

### 刪除容器後，數據仍然保留

刪除容器：

```bash
docker rm -f my_container

```

查看數據：

```bash
docker run --rm -v my_volume:/app/data busybox ls /app/data

```

通過 Volume，確保容器刪除後，重要的數據依然安全且可訪問。