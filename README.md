# Web API for Semantic Textual Similarity
A Flask application (Web API with GUI) serving SBERT models for semantic textual similarity 

![image-info](https://github.com/SebastianKotstein/Web-API-for-Multilingual-Textual-Similarity/blob/master/images/Web-UI_2024_01_06.png)

## Web API and UI for Inference
We created a Flask app that enables the application of a RESTBERTa model through a Web API and UI.
To use this application, download this repository, navigate to [tools](https://github.com/SebastianKotstein/RESTBERTa/tree/master/tools), and create a docker image with:
```
docker build -t restberta-core .
```
Use one of the following commands to start the application. Set the ```MODEL``` parameter to specify the RESTBERTa model that should be loaded.

### Parameter Matching
Run the following command to start a container with the RESTBERTa model that has been fine-tuned to parameter matching exclusively:
```
docker run -d -p 80:80 -e MODEL=SebastianKotstein/restberta-qa-parameter-matching --name pm-cpu restberta-core
```
### Endpoint Discovery
Run the following command to start a container with the RESTBERTa model that has been fine-tuned to endpoint discovery exclusively:
```
docker run -d -p 80:80 -e MODEL=SebastianKotstein/restberta-qa-endpoint-discovery --name ed-cpu restberta-core
```
### Parameter Matching and Endpoint Discovery
Run the following command to start a container with the RESTBERTa model that has been fine-tuned to both tasks:
```
docker run -d -p 80:80 -e MODEL=SebastianKotstein/restberta-qa-pm-ed --name pm-ed-cpu restberta-core
```
### Custom Models
To start a container with a custom model hosted on Hugging Face, you can specify any model repository using the ```MODEL``` parameter. Additionally, if the model repository is private, the ```TOKEN``` parameter allows you to set a Hugging Face access token, e.g.:
```
docker run -d -p 80:80 -e MODEL=my-user/my-qa-model -e TOKEN=hf_12345678 --name my-model-cpu restberta-core
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

