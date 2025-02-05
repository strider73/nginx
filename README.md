# **Nginx Proxy Manager Setup & SSL Configuration**  

## **1. Setup and Configuration**  
This repository contains the configuration files for setting up **Nginx Proxy Manager** along with **MariaDB** using Docker.  

### **Docker Compose Setup**  
Ensure your `docker-compose.yml` file includes the correct services and environment variables:  

```yaml
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'    # Public HTTP Port
      - '443:443'  # Public HTTPS Port
      - '81:81'    # Admin Web Port
    expose:
      - '21:21'    # FTP Port
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
      DISABLE_IPV6: 'true' # Uncomment if IPv6 is not enabled
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./mysql:/var/lib/mysql
```

---

## **2. SSL Certificate Issue and Resolution**  

If you encounter an **SSL certification error** (e.g., expired certificates), ensure the following:  

- **Port 80 is open** on your internet gateway (e.g., `192.168.1.1`).  
- **Let’s Encrypt SSL certificates are renewed** properly.  
- Check your current certificate expiration dates using **Nginx Proxy Manager** under **SSL Certificates**.  

### **Example Expired Certificate Error**  
If your SSL certificates appear in red, like below, they have expired:  
[!image desc](./img/Screenshot%202024-12-24%20at%206.53.18 pm.png)

To renew, go to **Nginx Proxy Manager > SSL Certificates** and manually reissue the certificate or set up **auto-renewal**.  

---

## **3. Current Proxy Settings (Latest Update)**  

As of the most recent update, these are the configured **proxy hosts** in **Nginx Proxy Manager**:  

| Source | Destination | SSL | Access | Status |
|--------|------------|-----|--------|--------|
| `adventuretube.net` | `http://192.168.1.129:8020` | Let’s Encrypt | Public | 🟢 Online |
| `api.adventuretube.net` | `http://192.168.1.129:8030` | Let’s Encrypt | Public | 🟢 Online |
| `config.adventuretube.net` | `http://192.168.1.129:9297` | Let’s Encrypt | Public | 🟢 Online |
| `deepseek.adventuretube.net` | `http://192.168.1.116:3001` | Let’s Encrypt | Public | 🟢 Online |
| `eureka.adventuretube.net` | `http://192.168.1.129:8761` | Let’s Encrypt | Public | 🟢 Online |
| `gateway.adventuretube.net` | `http://192.168.1.129:8030` | Let’s Encrypt | Public | 🟢 Online |
| `jenkins.adventuretube.net` | `https://192.168.1.129:8443` | Let’s Encrypt | Public | 🟢 Online |
| `mongodb.adventuretube.net` | `http://192.168.1.129:8081` | Let’s Encrypt | Public | 🟢 Online |
| `n8n.adventuretube.net` | `http://192.168.1.129:5678` | Let’s Encrypt | Public | 🟢 Online |
| `n8n2.adventuretube.net` | `http://192.168.1.101:5678` | Let’s Encrypt | Public | 🟢 Online |
| `nginx.adventuretube.net` | `http://192.168.1.129:81` | Let’s Encrypt | Public | 🟢 Online |
| `pgadmin.adventuretube.net` | `http://192.168.1.129:5050` | Let’s Encrypt | Public | 🟢 Online |
| `pi2.adventuretube.net` | `http://192.168.1.102:8020` | Let’s Encrypt | Public | 🟢 Online |
| `portainer.adventuretube.net` | `https://192.168.1.129:9443` | Let’s Encrypt | Public | 🟢 Online |
| `portainer2.adventuretube.net` | `https://192.168.1.102:9443` | Let’s Encrypt | Public | 🟢 Online |
| `whisper.adventuretube.net` | `http://192.168.1.129:8000` | Let’s Encrypt | Public | 🟢 Online |

📌 *Make sure your DNS records point to the correct IP address (`192.168.1.129`).*  

---

## **4. Default Administrator Login**  
Once Nginx Proxy Manager is up, you can log in with:  

- **Email:** `admin@example.com`  
- **Password:** `changeme` (You will be asked to reset it.)  

---

### ✅ **Next Steps:**  
- [ ] Verify SSL certificates are renewed.  
- [ ] Ensure your `docker-compose.yml` is correct.  
- [ ] Restart the Nginx Proxy Manager container:  
  ```bash
  docker-compose down && docker-compose up -d
  ```  
- [ ] Update any proxy settings if necessary.  

---

## **Contributing & Issues**  
If you encounter issues, feel free to open an issue in this repository. Happy hosting! 🚀  

---

### **🔄 Updated on:** *[Insert Today’s Date]*  

---

This updated `README.md` includes your latest **proxy settings**, **docker-compose configuration**, and **SSL troubleshooting guide** based on your latest screenshot.  

### ✅ **Next Steps for You:**  
1. Copy this updated `README.md` into your **GitHub repository**.  
2. Commit and push the changes.  
3. Test your proxy settings to ensure everything is working smoothly.  

Let me know if you need any modifications! 🚀