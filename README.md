# Docker Compose Flask App - Documentation

## **Project Overview**
This project is a simple Flask web application containerized using Docker and orchestrated with Docker Compose. The application serves a static webpage along with a REST API endpoint.

## **Project Structure**
```
docker-compose-app/
│── app/
│   ├── static/
│   │   ├── style.css
│   ├── templates/
│   │   ├── index.html
│   ├── app.py
│   ├── requirements.txt
│   ├── Dockerfile
│── docker-compose.yml
```

## **Prerequisites**
Ensure you have the following installed on your system:
- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/) (Use Docker Compose V2)

## **Installation and Setup**

### **Step 1: Clone the Repository**
```sh
git clone https://github.com/your-repo/docker-compose-app.git
cd docker-compose-app
```

### **Step 2: Build and Run the Application**
```sh
docker compose up --build -d
```

### **Step 3: Access the Application**
- Web Application: [http://localhost:5000](http://localhost:5000)
- API Endpoint: [http://localhost:5000/api](http://localhost:5000/api)

## **Application Files**

### **1. app.py (Flask Application)**
```python
from flask import Flask, render_template, send_from_directory

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/api')
def api():
    return {"message": "Hello, Docker Compose!"}

@app.route('/static/<path:filename>')
def static_files(filename):
    return send_from_directory('static', filename)

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### **2. templates/index.html (Static Web Page)**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Docker Compose App</title>
    <link rel="stylesheet" href="{{ url_for('static_files', filename='style.css') }}">
</head>
<body>
    <div class="container">
        <h1>Welcome to My Dockerized Flask App</h1>
        <p>This is a simple static webpage served by Flask.</p>
        <a href="/api">Click here to see the API response</a>
    </div>
</body>
</html>
```

### **3. static/style.css (CSS File)**
```css
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #f4f4f4;
    padding: 50px;
}

.container {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.1);
}
```

### **4. Dockerfile (Containerizing the App)**
```dockerfile
FROM python:3.9

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

### **5. docker-compose.yml (Compose Configuration)**
```yaml
version: '3.8'

services:
  web:
    build: ./app
    ports:
      - "5000:5000"
    restart: always
```

## **Stopping and Removing Containers**
To stop the running containers:
```sh
docker compose down
```

To remove unused images, networks, and volumes:
```sh
docker system prune -a
```

## **Troubleshooting**

### **1. Docker Compose Version Issue**
**Error:** `KeyError: 'ContainerConfig'`
- **Cause:** You may be using Docker Compose V1, which is deprecated.
- **Solution:** Upgrade to Docker Compose V2:
  ```sh
  sudo apt install docker-compose-plugin
  docker compose version
  ```

### **2. Port Conflict Issue**
**Error:** `Bind for 0.0.0.0:5000 failed: port is already allocated`
- **Solution:** Stop the process using the port:
  ```sh
  sudo lsof -i :5000
  sudo kill -9 <PID>
  ```
  Then restart the containers:
  ```sh
  docker compose up -d
  ```

### **3. Container Not Running**
**Error:** `Exited (1)` when running `docker ps -a`
- **Solution:** Check logs:
  ```sh
  docker compose logs
  ```
  Fix any errors in `Dockerfile` or `app.py` based on logs.

### **4. Changes Not Reflecting**
**Solution:** Rebuild the container:
```sh
docker compose up --build -d
```

## **Next Steps**
- Add a database (MySQL/PostgreSQL) with Docker Compose.
- Integrate Nginx as a reverse proxy.
- Deploy the project to AWS, Azure, or GCP.

---
This documentation provides everything you need to set up, run, and troubleshoot the Dockerized Flask application. 🚀

