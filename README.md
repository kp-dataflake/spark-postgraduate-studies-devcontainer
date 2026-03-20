


# Szablon Projektu Spark

Lokalny klaster Apache Spark + środowisko JupyterLab obsługiwane przez Docker Compose.

## Zakres projektu

- Spark master pod adresem `spark://spark-master:7077`
- Dwa worker'y Spark
- JupyterLab skonfigurowany do połączenia z klastrem Spark
- Przykładowy notatnik w `notebooks/spark_session.ipynb`
- Przykład przesłania zadania Spark w `notebooks/spark_job_example` (komenda uruchomienia: `docker exec -it jupyter bash /home/jovyan/work/spark_job_example/launcher.sh`)

## Wymagania Wstępne

- Docker Desktop (lub Docker Engine + wtyczka Compose)
- Powłoka Bash

## Uruchomienie Usług

```bash
bash scripts/start-services.sh
```

Spowoduje to zbudowanie/uruchomienie usługi `jupyter` i przywołanie zależnych usług Spark.

## Zatrzymanie Usług

```bash
bash scripts/stop-services.sh
```

## Adresy URL Usług

- Interfejs Spark Master: [http://localhost:8080](http://localhost:8080)
- Interfejsy Pracowników Spark:
  - [http://localhost:8081](http://localhost:8081)
  - [http://localhost:8082](http://localhost:8082)
- JupyterLab: [http://localhost:8888](http://localhost:8888)
- Interfejs Zadania Spark (z przebiegów notatnika): [http://localhost:4040](http://localhost:4040)

## Uruchomienie Sesji Notatnika + spark-submit

W trybie Spark Standalone pierwsza aktywna aplikacja Spark może domyślnie zarezerwować wszystkie dostępne zasoby. Może to spowodować, że druga aplikacja będzie czekała z komunikatem:

`Initial job has not accepted any resources`


## Limity Zasobów (Domyślne)

Ten szablon celowo uruchamia mały lokalny klaster:

- `spark-worker-1`: `1` rdzeń, `1g` pamięci
- `spark-worker-2`: `1` rdzeń, `1g` pamięci
- Całkowita pojemność klastra: `2` rdzenie, `2g` pamięci

Implikacje:

- Uruchomienie wielu aplikacji Spark jednocześnie może kolejkować zadania, gdy pojemność się wyczerpie.
- Interaktywne sesje notatnika powinny być małe.
- W przypadku obciążeń cięższych lub równoległych zwiększ `SPARK_WORKER_CORES` i `SPARK_WORKER_MEMORY` w `docker/docker-compose.yml`.

## Dane i Notatniki

- Notatniki są montowane z `./notebooks` do `/home/jovyan/work` w kontenerze Jupyter.
- Lokalne dane można montować poprzez `./data` do `/home/jovyan/data`.
