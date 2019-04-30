# Airflow-on-GCP
Using Airflow with GCP composer

#### To create a new composer from the cloud shell.
$ gcloud beta composer environments create *first-airflow-test* --location europe-west1 --zone europe-west1-b --python-version=3

#### To see the airflow environments, do from cloud shell
$ gsutil cat gs://europe-west1-first-airflow--96d53b82-bucket/airflow.cfg

#### To install modules listed in requirements.txt, first upload the requirements.txt file. async means this will run in the background
$ gcloud composer environments update first-airflow-test --update-pypi-packages-from-file /home/gina524286/requirements.txt --location europe-west1 --async

#### To set up enivronment variables to be used in the dag files. 
$ gcloud composer environments run first-airflow-test --location europe-west1 variables -- --set *gcp_project* transfer-gcp
$ gcloud composer environments run first-airflow-test --location europe-west1 variables -- --set *gcs_source_bucket* gs://catalog_images_bkt
$ gcloud composer environments run first-airflow-test --location europe-west1 variables -- --set *gcs_dest_bucket* gs://catalog_images_bkt_backup

#### To list all the logs & dags
$ gsutil ls -r gs://europe-west1-first-airflow--96d53b82-bucket/logs|dags

#### To list all the buckets & delete a particular bucket
$ gsutil ls
$ gsutil rm -r gs://catalog_images_bkt gs://catalog_images_bkt_backup

#### To delete dags from cloud shell. The storage is there, since the python files/dags are in `Storage`
$ gcloud composer environments storage dags delete --environment <composer-name> --location <composer-location> <some-python-dag-file>
$ gcloud composer environments storage dags delete --environment first-airflow-test --location europe-west1 python_bash_dummy.py

#### To list composer environments, in a given location.
$ gcloud beta composer environments list --locations=europe-west1

#### To delete the composer
$ gcloud composer environments delete first-airflow-test --location europe-west1

#### Setting up the env variable for sendGrid. This can also be done via the UI.
$ gcloud composer environments update sendgrid-airflow-test --location europe-west1 --update-env-variables=SENDGRID_API_KEY= <some-api-key>
$ gcloud composer environments update sendgrid-airflow-test --location europe-west1 --update-env-variables=SENDGRID_MAIL_FROM=hahaha@gmail.com


# BigQuery stuff

#### To make a table in BigQuery given a schema. First upload the json file using gcloud console.
$ bq mk --schema product_table.json -t avid-infinity-196009:product_sendgrid.products_table
