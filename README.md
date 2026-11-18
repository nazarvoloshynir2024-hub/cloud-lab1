cat << 'EOF' > README.md


Навчальний проєкт із розгортання контейнеризованого REST API (FastAPI + PostgreSQL) у хмарному середовищі AWS за допомогою Terraform (IaC) та автоматичного деплою через GitHub Actions (CI/CD).



* **Swagger UI (документація API):** `http://<ALB_DNS_NAME>/docs` *(актуальний URL генерується командою `terraform output alb_public_url` перед захистом)*
* **Health Check:** `http://<ALB_DNS_NAME>/health`



* **Застосунок:** Python 3.12, FastAPI, SQLAlchemy, Uvicorn.
* **Контейнеризація:** Docker (multi-layer збірка на базі `python:3.12.6-slim-bookworm`, запуск від не-root користувача `appuser`) та Docker Compose для локального запуску.
* **Хмарна інфраструктура (AWS, Terraform):**
  * **VPC:** публічні та приватні підмережі у двох зонах доступності (`eu-north-1a`, `eu-north-1b`).
  * **ECR:** приватний реєстр Docker-образів.
  * **RDS PostgreSQL:** керована база даних у приватних підмережах без публічного доступу (Security Group приймає трафік лише від ECS).
  * **ECS Fargate + ALB:** запуск контейнера за Application Load Balancer з перевіркою стану `/health`.
* **CI/CD:** GitHub Actions автоматично збирає Docker-образ при push у `main`, пушить його в AWS ECR та оновлює сервіс ECS.



```text
.
├── .github/workflows/
│   └── deploy.yml          
├── app/
│   ├── main.py           
│   └── requirements.txt  
├── terraform/
│   ├── main.tf             
│   ├── variables.tf        
│   └── outputs.tf          
├── compose.yaml            
├── Dockerfile              
├── .gitignore              
└── README.md               