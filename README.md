# Prematurely Optimized Real-Time Dashboard

### Description
Personal optimizations made to the [Senior Design Lab 1 Dashboard](https://github.com/Senior-Design-2025-2026/L1-web-application) to explore new tools.

<div align="center">
  <img src="img/arch.png" alt="System Architecture" width="1000">
  <div><em>System Architecture</em></div>
</div>

---

### Project Tags
<div align="left">
  <img src="tags/personal.svg" width="123" alt="Personal" />
  <img src="tags/handwritten.svg" width="171" alt="Handwritten" />
  <img src="tags/python.svg" width="60" alt="Python" />
  <img src="tags/plotly.svg" width="60" alt="Plotly" />
  <img src="tags/redis.svg" width="60" alt="Redis" />
  <img src="tags/docker.svg" width="60" alt="Docker" />
  <img src="tags/postgres.svg" width="60" alt="Postgres" />
  <img src="tags/sqlalchemy.svg" width="60" alt="Postgres" />
  <img src="tags/flask.svg" width="60" alt="Flask" />
  <img src="tags/celery.svg" width="60" alt="Celery" />
</div>

---

## Optimizations Made
1. **Improved UI with Mantine Components:**
- previous: default dcc and dbc components
- new:
- - dash mantine components,
- - UIowa themed with light/dark toggle
- - Overall much cleaner and more intuitive UI/UX

2. **Decoupled Subscription and Visualization (Streamer App):**
- previous: streamer attached to dashboard directly; dashboard handles data subscription and data viz
- new:
- - implemented a streamer application (using redis streams) that sits between the IoT device(s) and dashboard.
- - allows for more devices to stream without clogging the dashboard task queue
- - connected via a UNIX socket

3. **Decoupled Emailing and DB I/O (Celery App):**
- previous: email alerts attached to dashboard directly; slight lag of application when email alerts are sent
- new:
- - implemented a celery task queue which schedules DB CRUD actions and automated emails on separate app
- - improved workflow to keep the data visualization app data visualization specific 
- - connected via a UNIX socket

4. **Database (Postgres):**
- previous: hardcoded email to send automated alerts to
- new:
- - implemented a postgres connection to allow for multiple users
- - each user can set their desired temperature ranges, receiving an alert at their registered email
- - allows for future addition of devices and historical storage

5. **Containeriation (Docker):**
- previous: no containers & dashboard tightly coupled
- new:
- - docker containers for dashboard, stream writer, postgres, and celery apps run on separate containers
- - psuedo parallelism achieved --> better performance of dashboard (which was my goal :) )

This project required reading LOTS of documentation. Before implementing these optimizations, I had only indirectly used docker, redis, sqlalchemy, and celery at John Deere. I challenged myself to learn each of these through handcoding an extension of an already completed project. Please see the images for before and afters. 
This project was impractical, which is why I am calling it the "Prematurely Optimized Real-Time Dashboard;" The original dashboard performed well enough and satisfied the requirements. Even so, I gained practical experience for thinking about and implementing optimizations by hand (as well as comfortability using new tools). 

## Project Images 

<div align="center">
  <img src="img/image-6.png" alt="Finished Product - Additional View" width="800">
  <div><em>Web Application</em></div>
</div>


<div align="center">
  <img src="img/schema.png" alt="application responsibility" width="400">
  <div><em>Tables</em></div>
</div>

<div align="center">
  <img src="img/image-10.png" alt="Finished Product - Additional View" width="800">
  <div><em>Embedded System</em></div>
  <br>
</div>

## Source Code

**Software Application:**
  - Web Application  
    - [web-application](web-application/README.md)
  - Streamer API  
    - [stream-writer](stream-writer/README.md)  
  - Asynchronous Task Queue  
    - [celery-worker](celery-worker/README.md)  
  - Sqlalchemy ORM  
    - [postres-orm](postgres-orm/README.md)
  
# View the Application Locally
You only need to have Docker installed on your computer to run the "DUMMY DATA MODE" of the application locally. Please read the [Docker documentation](https://docs.docker.com/) on installation.

## Running the Containers:
to start the application simply run the following command
```
docker compose up
```

## Viewing the Dashboard
Running the container will expose port 8000 which your mobile device can connect to. 

To find your devices IP, use the following command:

**MacOS**: `ipconfig getifaddr en0`

**Linux**: `ip addr show`

**Windows**: `ipconfig`

Then, within your mobile browser, enter the path:

```
<IP>:8000    
```
ex: 127.0.0.1:8000 (this can be used for viewing on machine running the containers)

_(alternatively, you can open the computer running the container but the CSS is not configured for larger screens and will not look as nice)_


<div align="center">
  <img src="img/image-7.png" alt="Finished Product - Additional View" width="800">
  <div><em>Running Project</em></div>
</div>
