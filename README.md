# Web API for Semantic Textual Similarity
A Flask application that enables the application of an SBERT model for semantic textual similarity through a Web API and UI.

![image-info](https://github.com/SebastianKotstein/Web-API-for-Semantic-Textual-Similarity/blob/master/images/Web-UI_2024_01_06.png)

## Run Flask App
To use this application, download this repository and create a docker image with:
```
docker build -t sbert-flask .
```
Use the docker ```run``` command to start the application. Specify the SBERT model that should be loaded by setting the ```MODEL```parameter, e.g.:
```
docker run -d -p 80:80 -e MODEL=sentence-transformers/all-MiniLM-L6-v2 --name sbert-all-minilm-l6-v2 sbert-flask
```
If you need to run a model hosted on Hugging Face in a private repository, you can specify a Hugging Face access token using the ```TOKEN``` parameter:
```
docker run -d -p 80:80 -e MODEL=my-user/my-sbert-model -e TOKEN=hf_12345678 --name my-sbert-model sbert-flask
```
### Web UI
To use the Web UI, open a browser and navigate to http://localhost:80.

### Web API
The Web API is exposed over the same endpoints as the Web UI. For proper routing of HTTP requests to the Web API, it is therefore essential to set the ```Accept``` and ```Content-Type``` headers and
specify either ```application/json``` or one of the application-specific MIME-types (see OpenAPI documentation) as request and response format.
Use the following cURL to make a prediction:
```
curl -L 'http://localhost:80/predict' \
-H 'Accept: application/vnd.skotstein.restberta-core.results.v1+json' \
-H 'Content-Type: application/vnd.skotstein.restberta-core.schemas.v1+json' \
-d ' {
 "schemas":[
    {
      "schemaId": "s00",
      "name": "My schema",
      "value": "state units auth.key location.city location.city_id location.country location.lat location.lon location.postal_code",
      "queries": [
        {
          "queryId": "q0",
          "name": "My query",
          "value": "The ZIP of the city",
          "verboseOutput": true
        }
      ]
    }
  ]
}'
```

