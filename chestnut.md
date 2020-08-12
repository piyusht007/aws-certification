ProjectController -> Put message into `chestnut-new-project`

(Consumer) WorkInitMessageHandler#**handleNewProject**

- If **startWithOrder7**
    - Publish a message to **chestnut-order7-processing**
    - Publish a message to **chestnut-order7-metadata**
    - Publish a message to **chestnut-process-completion** with type `ORDER7_EXTRACT`

- If **startWithOrder1**
    - Publish a message to **chestnut-order1-processing**
    - Publish a message to **chestnut-order1-metadata**
    - Publish a message to **chestnut-process-completion** with type `ORDER1_EXTRACT`

- If **startWithCountry**
    - Publish a message to **chestnut-country-processing**

(Consumer) WorkInitMessageHandler#**processCountryMessage**

- splitCountryToOrder1Areas
    - For each **Order1** 
        - Publish a message to **chestnut-order1-processing**
        - Publish a message to **chestnut-order1-metadata**
        - Publish a message to **chestnut-process-completion** with type `ORDER1_EXTRACT`

(Consumer) WorkInitMessageHandler#**processOrder1Message**

- splitOrder1Areas
    - splitOrder1ToOrder8Areas
        - For each Order8
            - Publish a message to **chestnut-order8-processing**
            - Publish a message to **chestnut-order8-metadata**
            - Publish a message to **chestnut-process-completion** with type `ORDER8_EXTRACT`
 
    - splitOrder1ToOrder7Areas
        - For each Order8
            - Publish a message to chestnut-order7-processing
            - Publish a message to chestnut-order7-metadata
            - Publish a message to chestnut-process-completion with type `ORDER7_EXTRACT`

(Consumer) WorkInitMessageHandler#**processOrder7Message**

- splitOrder7ToOrder8Areas
    - For each Order8
        - Publish a message to chestnut-order8-processing
        - Publish a message to chestnut-order8-metadata
        - Publish a message to chestnut-process-completion with type `ORDER8_EXTRACT`


**Order8 message consumers:**


(Consumer) CityNameServiceMessageHandler#**processOrder8Message**

- Start city extraction with order8Id
- projectProgressService.incrementProcessedCount


CityNameServiceMessageHandler#**processProcessCompletionMessage**

- handleProcessCompletionMessage for upstreamCompletionType i.e. `Order8_EXTRACT`
    - projectProgressService#handleProcessCompletionMessage
        - START a CompletionServiceMonitor thread to check if city extraction is complete by consulting with projectProgressService, if expected numbers of messages are processed then,
        - Publish a message to chestnut-process-completion with type `CITY_NAME_EXTRACT` 
- handleProcessCompletionMessage for upstreamCompletionType i.e. `CITY_NAME_EXTRACT`
    - Mark the project as completed


RoadElementServiceMessageHandler#**processOrder8Message**

- Start road element extraction with order8Id
- projectProgressService.incrementProcessedCount

RoadElementServiceMessageHandler#**processProcessCompletionMessage**

- handleProcessCompletionMessage for upstreamCompletionType i.e. `Order8_EXTRACT`
- projectProgressService#handleProcessCompletionMessage
- START a CompletionServiceMonitor thread to check if city extraction is complete by consulting with projectProgressService, if expected numbers of messages are processed then,
- Publish a message to chestnut-process-completion with type `ROAD_ELEMENT_EXTRACT` 
- handleProcessCompletionMessage for upstreamCompletionType i.e. `ROAD_ELEMENT_EXTRACT`
- Mark the project as completed

PostalPointServiceMessageHandler#**processOrder8Message**

- Start city extraction with order8Id
- projectProgressService.incrementProcessedCount
									
PostalPointServiceMessageHandler#**processProcessCompletionMessage**

- handleProcessCompletionMessage for upstreamCompletionType i.e. `Order8_EXTRACT`
    - projectProgressService#handleProcessCompletionMessage
        - START a CompletionServiceMonitor thread to check if city extraction is complete by consulting with projectProgressService, if expected numbers of messages are processed then,
        - Publish a message to chestnut-process-completion with type `POSTAL_POINT_EXTRACT` 
- handleProcessCompletionMessage for upstreamCompletionType i.e. `POSTAL_POINT_EXTRACT` 
    - Mark the project as completed