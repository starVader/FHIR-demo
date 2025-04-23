# This repository helps setup a local FHIR server for learning

# Setup and Run
* Docker, docker-compose, Java and git are required running this repository.
* Run: $ docker run -d -p 8080:8080 hapiproject/hapi:latest
* Use Postman or other tool to open 
  * http://localhost:8080/
  * http://localhost:8080/fhir
* Validate Schema:
  * java -jar validator_cli.jar patient.json -version 4.0.1

# Usage
* Create a Patient resource
  ```
  curl --location 'http://localhost:8080/fhir/Patient' \
  --header 'Content-Type: application/json' \
    --data '{
    "resourceType": "Patient",
    "name": [
      {
      "use": "official",
      "family": "Doe",
      "given": [
        "John"
        ]
      }
    ],
    "gender": "male",
    "birthDate": "1985-02-20"
    }'
  ```
* Find Patient resource by ID
  ```
  curl --location 'http://localhost:8080/fhir/Patient/1'
  ```
  
  * Search Patient resource by name
    ```
    curl --location 'http://localhost:8080/fhir/Patient?name=John'
    ```
  
* Update Patient resource
  ```
  curl --location --request PUT 'http://localhost:8080/fhir/Patient/1' \
  --header 'Content-Type: application/json' \
  --data '{
  "resourceType": "Patient",
  "id": "1",
  "meta": {
  "profile": [
    "http://hl7.org/fhir/us/core/StructureDefinition/us-core-patient"
  ]
  },
  "name": [
    {
      "use": "official",
      "family": "Doe",
      "given": [
      "John"
          ]
    }
  ],
  "gender": "male",
  "birthDate": "1985-02-20",
  "address": [
   {
     "use": "home",
     "line": [
     "123 Main Street"
     ],
  "city": "Metropolis",
  "state": "NY",
  "postalCode": "12345",
  "country": "USA"
    }
   ]
  }'
  ```