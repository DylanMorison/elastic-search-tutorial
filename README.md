# Elastic Stack Udemy Course Notes

Course URL: https://www.udemy.com/course/elasticsearch-complete-guide/learn/lecture/7373340?start=0#overview

All images taken directly from Bo Andersen's lecture slides.

| Action                 | Date    |
| ---------------------- | ------- |
| Started Course         | 8/29/22 |
| Completed Lectures 1-X | 8/29/22 |

## Lecture 3: Elastic Stack:

-   **Elastic Search**
-   **Kibana**
-   **Logstash**: A data processing pipeline. It receives events, processes/filters them, and sends them to one or more platforms. Defined in a proprietary markup language.
-   **X-Pack**: Adds additional features to the Elasticsearch & kibana. The most important of these features include:
    -   **Security**: Adds authentication and authorization to elasticsearch and kibana. Controls user permissions. Different people might need different privileges.
    -   **Monitoring**: Gain insight into how the elastic stack is running.
    -   **Alerting**: Check if CPU usage is too high, or errors starting propagating.
    -   **Reporting**
    -   **Machine Learning**: Abnormality Detection, Forecasting, etc...
    -   **Graphs**: Analyses relationships between data.
    -   **SQL**: Elasticsearch queries are written in Query DSL. It is flexible but verbose.
-   **Beats**

### Summary of Elastic Stack

The center of it all is elastic search, which contains the data. Injecting data into elastic search can be done with **beats** or **elastic stash**, as well as through elasticsearch's api. **Kibana** is a UI that sits on top of elasticsearch to let you visualize the data that you receive. **X-pack** enables additional features such as ML.

ELK Stack = `E`lasticsearch + `L`ogstash + `K`ibana
This term originates from before X-pack existed. The `elastic stack` is a superset of the `ELK` stack.

## Lecture 4: Walk-through of common architectures

Suppose we have an E-commerce app. Our data is stored in a relational db such as postgres.

![hi](./course-diagrams/asdasd.png)

We want to improve the search functionality of this app. So far it has been directly using the postgres db to get search info, but this is inefficient and not what dbs are for. Elasticsearch is _much_ better for this. When a user types in a search from the e-commerce frontend, the request is sent directly to elasticsearch. This can be done with an HTTP req, however:

![get search info from elasticsearch](course-diagrams/Screenshot%20from%202022-07-29%2015-31-53.png)

But how do we get data into elasticsearch in the first place? And how do we keep it updated? We will do this with data duplication from our postgres db. If a user adds a new product though, how do we get it into elasticsearch? You will need to write a script that imports that data. From the moment the data is imported elasticsearch will keep it updated. This is the simplistic usage of elasticsearch.
<br>

Now, what if we want to implement a UI for elasticsearch? We would use kibana for this.

![kibana](course-diagrams/Screenshot%20from%202022-07-29%2015-37-38.png)

We will need to spin up a dedicated server to run kibana and configure it with elasticsearch. Overtime, if web traffic increases, the server will start to sweat. We will need to monitor server resources with `Metricbeat` by installing it on the kibana server. But how does the Metricbeat data get into elasticsearch? We can simply configure metricbeat to send data to elasticsearch via an ingest node. The details of this is not important right now. Now that system metrics are being stored in elastic search we can visualize the metric data in kibana. Metricbeat has a default dashboard within kibana.

We want to monitor access logs and error logs now too. We can even check response times for each endpoint. This allows us to identify bad deployments. `Filebeat` can be used for this task.

Next, fast forward 6 months. We have added a ton more code, functionality, and products to the e-commerce app. We will want to wire up `Logstash` for event processing.  

![Logstash](course-diagrams/Screenshot%20from%202022-07-29%2015-48-44.png)
