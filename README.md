# Day-74----Node-Exporter-cAdvisor-and-Grafana-Dashboards

# Task
Prometheus is running and you can query metrics. But right now it is only monitoring itself. In production, you need to monitor two critical things: the host machine (CPU, memory, disk, network) and the Docker containers running on it.

Today you add Node Exporter for host metrics, cAdvisor for container metrics, and set up Grafana to visualize everything in dashboards instead of raw PromQL.

# Expected Output

- Node Exporter running and scraped by Prometheus
- cAdvisor running and scraped by Prometheus
- Grafana running with Prometheus configured as a datasource
- At least one custom Grafana dashboard with CPU, memory, and container panels
- A markdown file: day-74-exporters-grafana.md

# Challenge Tasks

# Task 1: Add Node Exporter for Host Metrics

Node Exporter exposes Linux system metrics (CPU, memory, disk, filesystem, network) in Prometheus format.

Update your docker-compose.yml from Day 73 -- add the Node Exporter service:

<img width="660" height="302" alt="image" src="https://github.com/user-attachments/assets/24446080-b0bf-461b-a91e-fede0bc4e2de" />

Why these volume mounts?

- /proc -- kernel and process information (CPU stats, memory info)
- /sys -- hardware and driver details
- / -- filesystem usage (disk space)
  
All mounted read-only (ro) -- Node Exporter only reads, never modifies.

Add it as a scrape target in prometheus.yml:

<img width="433" height="166" alt="image" src="https://github.com/user-attachments/assets/d0890f55-7b8b-4b0a-9e70-148e7d2005a4" />

Restart the stack:

<img width="242" height="50" alt="image" src="https://github.com/user-attachments/assets/4c543b3c-3b74-4259-9c8f-cfe621f4aa42" />

Verify Node Exporter is healthy:

<img width="439" height="45" alt="image" src="https://github.com/user-attachments/assets/3b77ddaf-2550-4d4d-a6ce-64ac1ba89646" />

Check Prometheus Targets page -- node-exporter should show as UP.

Run these queries in Prometheus to see host metrics:

<img width="604" height="287" alt="image" src="https://github.com/user-attachments/assets/422e2b29-d5b3-44f9-8d81-cac664eb3d94" />

<img width="957" height="336" alt="Image" src="https://github.com/user-attachments/assets/dd62644c-8873-49b9-a48d-009ceb6fa0ff" />
<img width="1110" height="454" alt="Image" src="https://github.com/user-attachments/assets/c5d801b6-6774-4bcc-8b29-a164c3c1e7f1" />
<img width="1110" height="410" alt="Image" src="https://github.com/user-attachments/assets/bac64fdb-7a34-4ea9-9dfc-7b4ead847e39" />
<img width="1122" height="354" alt="Image" src="https://github.com/user-attachments/assets/0ddbe1c2-8426-430b-81fb-b1949c39b1be" />
<img width="1115" height="381" alt="Image" src="https://github.com/user-attachments/assets/0f75f6bb-e1f8-46d5-85c0-153a7f831c50" />
<img width="1120" height="451" alt="Image" src="https://github.com/user-attachments/assets/12fe326a-bdf3-42c5-9953-b7affcff79ba" />
<img width="1117" height="393" alt="Image" src="https://github.com/user-attachments/assets/a68e7b48-2c57-4941-832b-775c21e63cd6" />


# Task 2: Add cAdvisor for Container Metrics

cAdvisor (Container Advisor) monitors resource usage and performance of running Docker containers.

Add it to your docker-compose.yml:

<img width="460" height="206" alt="image" src="https://github.com/user-attachments/assets/a4c42c1a-9437-4956-88ea-54a10e120476" />

Why these volume mounts?

- Docker socket (docker.sock) -- lets cAdvisor discover and query running containers
- /sys -- kernel-level container stats (cgroups)
- /var/lib/docker/ -- container filesystem information

Add cAdvisor as a Prometheus scrape target:

<img width="374" height="90" alt="image" src="https://github.com/user-attachments/assets/c10d3974-ff98-4269-9a69-aaa818e1f357" />

Restart and verify:

docker compose up -d

Open http://localhost:8080 to see the cAdvisor web UI. Click on Docker Containers to see per-container stats.

Run these queries in Prometheus:

<img width="708" height="267" alt="image" src="https://github.com/user-attachments/assets/197f0ed3-f60c-45f5-8dc4-794e81b14118" />

<img width="934" height="918" alt="Image" src="https://github.com/user-attachments/assets/3fd12129-7022-452a-915b-0215063c9ea9" />
<img width="904" height="908" alt="Image" src="https://github.com/user-attachments/assets/fcf66104-384d-4f71-b9c6-4845832d73b2" />
<img width="746" height="709" alt="Image" src="https://github.com/user-attachments/assets/aa5ef2f6-005b-4a8f-89bd-20ec1bb1b699" />
<img width="926" height="338" alt="Image" src="https://github.com/user-attachments/assets/8b001128-4482-40f7-942d-b87dddb37522" />
<img width="942" height="420" alt="Image" src="https://github.com/user-attachments/assets/1a77ac81-5bda-4544-a7f7-d85327e5b895" />
<img width="962" height="301" alt="Image" src="https://github.com/user-attachments/assets/dc6fbbc5-37c6-4cb1-a19d-abb24c426ef6" />
<img width="940" height="329" alt="Image" src="https://github.com/user-attachments/assets/c8cbf493-20b8-4173-899a-c659841f273b" />
<img width="924" height="332" alt="Image" src="https://github.com/user-attachments/assets/23b00a8c-f4fd-4afb-8eee-6953ed580c0a" />






