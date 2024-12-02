
| Feature                  | `docker-compose stop`          | `docker-compose down`          |
|--------------------------|--------------------------------|--------------------------------|
| **Containers**           | Stopped but preserved          | Stopped and removed            |
| **Volumes**              | Preserved                      | Removed (unless explicitly persisted) |
| **Networks**             | Preserved                      | Removed (unless explicitly persisted) |
| **Restart**              | `docker-compose start`         | `docker-compose up`            |
| **Configuration**        | Preserved                      | Needs recreation               |
