# airflow-coolify
MLOPs Pipeline
```bash
# Spin up stack locally
cp .env.example .env
export AIRFLOW_UID=$(id -u)
docker compose up airflow-webserver airflow-scheduler
open http://localhost:8080
---

## 3  Local Development Workflow

1. **Clone** → `git clone https://github.com/<you>/my-mlops-airflow.git`
2. **Bootstrap** → `cp .env.example .env && export AIRFLOW_UID=$(id -u)`
3. **Run** → `docker compose up -d`
4. **Add DAGs** → place Python scripts in `dags/`, they auto‑refresh.
5. **Commit & Push** → Coolify auto‑deploys in < 2 min.

---

## 4  Secrets & Volumes Cheatsheet

| Need | How to store | Where it surfaces |
| ---- | ------------ | ----------------- |
| Fernet & webserver keys | Coolify → *Secrets* | `docker-compose.yaml` via `${…}` |
| Cloud creds (AWS, GCP) | Coolify → *Secrets* | DAG env, providers |
| DAG code & logs | `volumes: dags/logs` | Host path (survives redeploy) |

---

## 5  Quality Gates Checklist ✅

- [ ] `airflow dags list` passes locally and in CI.
- [ ] `.env.example` in repo, real `.env` ignored.
- [ ] `docker-compose.yaml` pins versions (Airflow, Postgres, Redis).
- [ ] Named volumes for `dags / logs / plugins`.
- [ ] `README.md` documents local bootstrap.
- [ ] GitHub → Coolify webhook configured.

---

## 6  Next Step → Hook to Coolify

Once the repo structure passes CI and secrets are loaded into Coolify, create a **Docker Compose resource** in Coolify, point it to `docker-compose.yaml`, and hit **Deploy**. Your DAGs will appear at `<coolify-domain>:8080`.

> Need branch previews? Enable *Preview Deployments* in the Coolify resource settings so each PR spins up an isolated Airflow instance.

---

### 📎  Useful References
- [Official Airflow Docker Guide](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html)
- [Coolify Docs → Docker Compose](https://docs.coolify.io/configuration/docker-compose)
- [Airflow Provider Packages](https://airflow.apache.org/docs/apache-airflow-providers/index.html)
