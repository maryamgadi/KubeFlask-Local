# Mini Projet CI/CD - Flask + Docker + Kubernetes

Petit projet de demonstration CI/CD avec une application Flask, containerisee avec Docker, testee avec Pytest et deployee sur Kubernetes (Minikube).

## Description

L'application expose une seule route HTTP:

- `GET /` -> retourne `Hello DevOps CI/CD`

## Stack technique

- Python 3.10
- Flask
- Pytest
- Docker
- Kubernetes (Minikube)
- GitHub Actions

## Structure du projet

```text
mini-projet-ci-cd/
├── app.py
├── test_app.py
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── deployment.yaml
├── service.yaml
└── .github/workflows/ci-cd.yml
```

## Lancer en local (Python)

```bash
python -m venv .venv
# Windows PowerShell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python app.py
```

Application disponible sur:

- `http://127.0.0.1:5000`

## Lancer avec Docker

```bash
docker build -t flask-app .
docker run -p 5000:5000 flask-app
```

Application disponible sur:

- `http://127.0.0.1:5000`

## Lancer avec Docker Compose

```bash
docker compose up --build
```

Application disponible sur:

- `http://127.0.0.1:5000`

## Tests

```bash
pytest
```

## Deploiement Kubernetes (Minikube)

Demarrer Minikube:

```bash
minikube start
```

Appliquer les manifests:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

Verifier l'etat:

```bash
kubectl get pods
kubectl get svc
```

## Acces a l'application sur Windows + Minikube (driver Docker)

Sur Windows avec le driver Docker, l'IP `minikube ip` n'est pas toujours directement joignable depuis le navigateur.

Utiliser une de ces 2 methodes:

1. URL fournie par Minikube

```bash
minikube service flask-service --url
```

2. Port-forward (simple et fiable)

```bash
kubectl port-forward service/flask-service 5000:5000
```

Puis ouvrir:

- `http://127.0.0.1:5000`

Important: garder le terminal ouvert pendant `minikube service --url` ou `kubectl port-forward`.

## Pipeline GitHub Actions (CI/CD)

Le workflow est dans `.github/workflows/ci-cd.yml`.

Declenchement:

- a chaque `push` sur la branche `main`

Etapes:

- checkout du code
- installation Python 3.10
- installation des dependances
- execution des tests Pytest
- login Docker Hub
- build + push de l'image Docker

Secrets GitHub requis (Repository settings -> Secrets and variables -> Actions):

- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`

## Ameliorations possibles

- ajouter une route `/health` pour le monitoring
- ajouter un ingress Kubernetes
- versionner les images Docker avec un tag Git commit
- ajouter un badge GitHub Actions dans ce README
