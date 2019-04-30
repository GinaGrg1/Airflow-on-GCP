# Airflow-on-GCP
Using Airflow with GCP composer

#### To create a new composer from the cloud shell.
$ gcloud beta composer environments create *first-airflow-test* --location europe-west1 --zone europe-west1-b --python-version=3

#### To see the airflow environments, do from cloud shell
$ gsutil cat gs://europe-west1-first-airflow--96d53b82-bucket/airflow.cfg

#### To install modules listed in requirements.txt, first upload the requirements.txt file. async means this will run in the background
$ gcloud composer environments update first-airflow-test --update-pypi-packages-from-file /home/gina524286/requirements.txt --location europe-west1 --async

#### To set up enivronment variables to be used in the dag files. 
$ gcloud composer environments run first-airflow-test --location europe-west1 variables -- --set gcp_project transfer-gcp
$ gcloud composer environments run first-airflow-test --location europe-west1 variables -- --set gcs_source_bucket gs://catalog_images_bkt
$ gcloud composer environments run first-airflow-test --location europe-west1 variables -- --set gcs_dest_bucket gs://catalog_images_bkt_backup

