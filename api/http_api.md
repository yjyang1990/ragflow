
# HTTP API Reference

## Create dataset

**POST** `/api/v1/dataset`

Creates a dataset.

### Request

- Method: POST
- URL: `http://{address}/api/v1/dataset`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
- Body:
  - `"id"`: `string`
  - `"name"`: `string`
  - `"avatar"`: `string`
  - `"tenant_id"`: `string`
  - `"description"`: `string`
  - `"language"`: `string`
  - `"embedding_model"`: `string`
  - `"permission"`: `string`
  - `"document_count"`: `integer`
  - `"chunk_count"`: `integer`
  - `"parse_method"`: `string`
  - `"parser_config"`: `Dataset.ParserConfig`

#### Request example

```bash
# "id": id must not be provided.
# "name": name is required and can't be duplicated.
# "tenant_id": tenant_id must not be provided.
# "embedding_model": embedding_model must not be provided.
# "navie" means general.
curl --request POST \
  --url http://{address}/api/v1/dataset \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}' \
  --data '{
  "name": "test",
  "chunk_count": 0,
  "document_count": 0,
  "parse_method": "naive"
}'
```

#### Request parameters

- `"id"`: (*Body parameter*)  
    The ID of the created dataset used to uniquely identify different datasets.  
    - If creating a dataset, `id` must not be provided.

- `"name"`: (*Body parameter*)  
    The name of the dataset, which must adhere to the following requirements:  
    - Required when creating a dataset and must be unique.
    - If updating a dataset, `name` must still be unique.

- `"avatar"`: (*Body parameter*)  
    Base64 encoding of the avatar.

- `"tenant_id"`: (*Body parameter*)  
    The ID of the tenant associated with the dataset, used to link it with specific users.  
    - If creating a dataset, `tenant_id` must not be provided.
    - If updating a dataset, `tenant_id` cannot be changed.

- `"description"`: (*Body parameter*)  
    The description of the dataset.

- `"language"`: (*Body parameter*)  
    The language setting for the dataset.

- `"embedding_model"`: (*Body parameter*)  
    Embedding model used in the dataset to generate vector embeddings.  
    - If creating a dataset, `embedding_model` must not be provided.
    - If updating a dataset, `embedding_model` cannot be changed.

- `"permission"`: (*Body parameter*)  
    Specifies who can manipulate the dataset.

- `"document_count"`: (*Body parameter*)  
    Document count of the dataset.  
    - If updating a dataset, `document_count` cannot be changed.

- `"chunk_count"`: (*Body parameter*)  
    Chunk count of the dataset.  
    - If updating a dataset, `chunk_count` cannot be changed.

- `"parse_method"`: (*Body parameter*)  
    Parsing method of the dataset.  
    - If updating `parse_method`, `chunk_count` must be greater than 0.

- `"parser_config"`: (*Body parameter*)  
    The configuration settings for the dataset parser.

### Response

The successful response includes a JSON object like the following:

```json
{
    "code": 0,
    "data": {
        "avatar": null,
        "chunk_count": 0,
        "create_date": "Thu, 10 Oct 2024 05:57:37 GMT",
        "create_time": 1728539857641,
        "created_by": "69736c5e723611efb51b0242ac120007",
        "description": null,
        "document_count": 0,
        "embedding_model": "BAAI/bge-large-zh-v1.5",
        "id": "8d73076886cc11ef8c270242ac120006",
        "language": "English",
        "name": "test_1",
        "parse_method": "naive",
        "parser_config": {
            "pages": [
                [
                    1,
                    1000000
                ]
            ]
        },
        "permission": "me",
        "similarity_threshold": 0.2,
        "status": "1",
        "tenant_id": "69736c5e723611efb51b0242ac120007",
        "token_num": 0,
        "update_date": "Thu, 10 Oct 2024 05:57:37 GMT",
        "update_time": 1728539857641,
        "vector_similarity_weight": 0.3
    }
}
```

- `"error_code"`: `integer`  
  `0`: The operation succeeds.

  
The error response includes a JSON object like the following:

```json
{
    "code": 102,
    "message": "Duplicated knowledgebase name in creating dataset."
}
```

## Delete datasets

**DELETE** `/api/v1/dataset`

Deletes datasets by ids.

### Request

- Method: DELETE
- URL: `http://{address}/api/v1/dataset`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
  - Body:
    - `"ids"`: `List[string]`


#### Request example

```bash
# Either id or name must be provided, but not both.
curl --request DELETE \
  --url http://{address}/api/v1/dataset \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}' \
  --data '{
  "ids": ["test_1", "test_2"]
  }'
```

#### Request parameters

- `"ids"`: (*Body parameter*)
    Dataset IDs to delete.


### Response

The successful response includes a JSON object like the following:

```json
{
    "code": 0 
}
```

- `"error_code"`: `integer`  
  `0`: The operation succeeds.

  
The error response includes a JSON object like the following:

```json
{
    "code": 102,
    "message": "You don't own the dataset."
}
```

## Update dataset

**PUT** `/api/v1/dataset/{dataset_id}`

Updates a dataset by its id.

### Request

- Method: PUT
- URL: `http://{address}/api/v1/dataset/{dataset_id}`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
  - Body: (Refer to the "Create Dataset" for the complete structure of the request body.)


#### Request example

```bash
# "id":  id is required.
# "name": If you update name, it can't be duplicated.
# "tenant_id": If you update tenant_id, it can't be changed
# "embedding_model": If you update embedding_model, it can't be changed.
# "chunk_count": If you update chunk_count, it can't be changed.
# "document_count": If you update document_count, it can't be changed.
# "parse_method": If you update parse_method, chunk_count must be 0. 
# "navie" means general.
curl --request PUT \
  --url http://{address}/api/v1/dataset/{dataset_id} \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}' \
  --data '{
  "name": "test",
  "tenant_id": "4fb0cd625f9311efba4a0242ac120006",
  "embedding_model": "BAAI/bge-zh-v1.5",
  "chunk_count": 0,
  "document_count": 0,
  "parse_method": "navie"
}'
```

#### Request parameters
(Refer to the "Create Dataset" for the complete structure of the request parameters.)


### Response

The successful response includes a JSON object like the following:

```json
{
    "code": 0 
}
```

- `"error_code"`: `integer`  
  `0`: The operation succeeds.

  
The error response includes a JSON object like the following:

```json
{
    "code": 102,
    "message": "Can't change tenant_id."
}
```

## List datasets

**GET** `/api/v1/dataset?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id}`

List all datasets

### Request

- Method: GET
- URL: `http://{address}/api/v1/dataset?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id}`
- Headers:
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'


#### Request example

```bash
# If no page parameter is passed, the default is 1
# If no page_size parameter is passed, the default is 1024
# If no order_by parameter is passed, the default is "create_time"
# If no desc parameter is passed, the default is True
curl --request GET \
  --url http://{address}/api/v1/dataset?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id} \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
```

#### Request parameters

- `path`: (*Path parameter*)
    The current page number to retrieve from the paginated data. This parameter determines which set of records will be fetched.
- `path_size`: (*Path parameter*)
    The number of records to retrieve per page. This controls how many records will be included in each page. 
- `orderby`: (*Path parameter*)
    The field by which the records should be sorted. This specifies the attribute or column used to order the results.
- `desc`: (*Path parameter*)
    A boolean flag indicating whether the sorting should be in descending order.
- `name`: (*Path parameter*)
    Dataset name
- `"id"`: (*Path parameter*)  
    The ID of the dataset to be retrieved.
- `"name"`: (*Path parameter*)  
    The name of the dataset to be retrieved.

### Response

The successful response includes a JSON object like the following:

```json
{
    "code": 0,
    "data": [
        {
            "avatar": "",
            "chunk_count": 59,
            "create_date": "Sat, 14 Sep 2024 01:12:37 GMT",
            "create_time": 1726276357324,
            "created_by": "69736c5e723611efb51b0242ac120007",
            "description": null,
            "document_count": 1,
            "embedding_model": "BAAI/bge-large-zh-v1.5",
            "id": "6e211ee0723611efa10a0242ac120007",
            "language": "English",
            "name": "mysql",
            "parse_method": "knowledge_graph",
            "parser_config": {
                "chunk_token_num": 8192,
                "delimiter": "\\n!?;。；！？",
                "entity_types": [
                    "organization",
                    "person",
                    "location",
                    "event",
                    "time"
                ]
            },
            "permission": "me",
            "similarity_threshold": 0.2,
            "status": "1",
            "tenant_id": "69736c5e723611efb51b0242ac120007",
            "token_num": 12744,
            "update_date": "Thu, 10 Oct 2024 04:07:23 GMT",
            "update_time": 1728533243536,
            "vector_similarity_weight": 0.3
        }
    ]
}
```

  
The error response includes a JSON object like the following:

```json
{
    "code": 102,
    "message": "The dataset doesn't exist"
}
```

## Upload files to a dataset

**POST** `/api/v1/dataset/{dataset_id}/document`

Uploads files to a dataset. 

### Request

- Method: POST
- URL: `/api/v1/dataset/{dataset_id}/document`
- Headers:
  - 'Content-Type: multipart/form-data'
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
- Form:
  - 'file=@{FILE_PATH}'

#### Request example

```shell
curl --request POST \
     --url http://{address}/api/v1/dataset/{dataset_id}/document \
     --header 'Content-Type: multipart/form-data' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}' \
     --form 'file=@test.txt'
```

#### Request parameters

- `"dataset_id"`: (*Path parameter*)
    The dataset id
- `"file"`: (*Body parameter*)  
    The file to upload

### Response

The successful response includes a JSON object like the following:

```shell
{
    "code": 0 
}
```

- `"error_code"`: `integer`  
  `0`: The operation succeeds.

  
The error response includes a JSON object like the following:

```shell
{
    "code": 3016,
    "message": "Can't connect database"
}
```

## Download a file from a dataset

**GET** `/api/v1/dataset/{dataset_id}/document/{document_id}`

Downloads files from a dataset. 

### Request

- Method: GET
- URL: `/api/v1/dataset/{dataset_id}/document/{document_id}`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
- Output:
  - '{FILE_NAME}'
#### Request example

```shell
curl --request GET \
     --url http://{address}/api/v1/dataset/{dataset_id}/document/{documents_id} \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --output '{FILE_NAME}'
```

#### Request parameters

- `"dataset_id"`: (*PATH parameter*)
    The dataset id
- `"documents_id"`: (*PATH parameter*)  
    The document id of the file.

### Response

The successful response includes a JSON object like the following:

```shell
{
    "code": 0 
}
```

- `"error_code"`: `integer`  
  `0`: The operation succeeds.

  
The error response includes a JSON object like the following:

```shell
{
    "code": 3016,
    "message": "Can't connect database"
}
```


## List files of a dataset

**GET** `/api/v1/dataset/{dataset_id}/info?keywords={keyword}&page={page}&page_size={limit}&orderby={orderby}&desc={desc}&name={name}`

List files to a dataset. 

### Request

- Method: GET
- URL: `/api/v1/dataset/{dataset_id}/info?keywords={keyword}&page={page}&page_size={limit}&orderby={orderby}&desc={desc}&name={name`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request GET \
     --url http://{address}/api/v1/dataset/{dataset_id}/info?keywords=rag&page=0&page_size=10&orderby=create_time&desc=yes \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
```

#### Request parameters

- `"dataset_id"`: (*PATH parameter*)
    The dataset id
- `keywords`: (*Filter parameter*)
    The keywords matches the search key workds;
- `page`: (*Filter parameter*)
    The current page number to retrieve from the paginated data. This parameter determines which set of records will be fetched.
- `page_size`: (*Filter parameter*)
    The number of records to retrieve per page. This controls how many records will be included in each page. 
- `orderby`: (*Filter parameter*)
    The field by which the records should be sorted. This specifies the attribute or column used to order the results.
- `desc`: (*Filter parameter*)
    A boolean flag indicating whether the sorting should be in descending order.
- `name`: (*Filter parameter*)
    File name.

### Response

The successful response includes a JSON object like the following:

```shell
{
    "code": 0,
    "data": {
        "docs": [
            {
                "chunk_count": 0,
                "create_date": "Wed, 18 Sep 2024 08:20:49 GMT",
                "create_time": 1726647649379,
                "created_by": "134408906b6811efbcd20242ac120005",
                "id": "e970a94a759611efae5b0242ac120004",
                "knowledgebase_id": "e95f574e759611efbc850242ac120004",
                "location": "Test Document222.txt",
                "name": "Test Document222.txt",
                "parser_config": {
                    "chunk_token_count": 128,
                    "delimiter": "\n!?。；！？",
                    "layout_recognize": true,
                    "task_page_size": 12
                },
                "parser_method": "naive",
                "process_begin_at": null,
                "process_duation": 0.0,
                "progress": 0.0,
                "progress_msg": "",
                "run": "0",
                "size": 46,
                "source_type": "local",
                "status": "1",
                "thumbnail": null,
                "token_count": 0,
                "type": "doc",
                "update_date": "Wed, 18 Sep 2024 08:20:49 GMT",
                "update_time": 1726647649379
            },
            {
                "chunk_count": 0,
                "create_date": "Wed, 18 Sep 2024 08:20:49 GMT",
                "create_time": 1726647649340,
                "created_by": "134408906b6811efbcd20242ac120005",
                "id": "e96aad9c759611ef9ab60242ac120004",
                "knowledgebase_id": "e95f574e759611efbc850242ac120004",
                "location": "Test Document111.txt",
                "name": "Test Document111.txt",
                "parser_config": {
                    "chunk_token_count": 128,
                    "delimiter": "\n!?。；！？",
                    "layout_recognize": true,
                    "task_page_size": 12
                },
                "parser_method": "naive",
                "process_begin_at": null,
                "process_duation": 0.0,
                "progress": 0.0,
                "progress_msg": "",
                "run": "0",
                "size": 46,
                "source_type": "local",
                "status": "1",
                "thumbnail": null,
                "token_count": 0,
                "type": "doc",
                "update_date": "Wed, 18 Sep 2024 08:20:49 GMT",
                "update_time": 1726647649340
            }
        ],
        "total": 2
    },
}
```

- `"error_code"`: `integer`  
  `0`: The operation succeeds.

  
The error response includes a JSON object like the following:

```shell
{
    "code": 3016,
    "message": "Can't connect database"
}
```

## Update a file information in dataset

**PUT** `/api/v1/dataset/{dataset_id}/info/{document_id}`

Update a file in a dataset

### Request

- Method: PUT
- URL: `/api/v1/dataset/{dataset_id}/document`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request PUT \
     --url http://{address}/api/v1/dataset/{dataset_id}/info/{document_id} \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --raw '{
         "document_id": "f6b170ac758811efa0660242ac120004", 
         "document_name": "manual.txt", 
         "thumbnail": null, 
         "knowledgebase_id": "779333c0758611ef910f0242ac120004", 
         "parser_method": "manual", 
         "parser_config": {"chunk_token_count": 128, "delimiter": "\n!?。；！？", "layout_recognize": true, "task_page_size": 12}, 
         "source_type": "local", "type": "doc", 
         "created_by": "134408906b6811efbcd20242ac120005", 
         "size": 0, "token_count": 0, "chunk_count": 0, 
         "progress": 0.0, 
         "progress_msg": "", 
         "process_begin_at": null, 
         "process_duration": 0.0
     }'
```

#### Request parameters

- `"document_id"`: (*Body parameter*)
- `"document_name"`: (*Body parameter*)

### Response

The successful response includes a JSON object like the following:

```shell
{
    "code": 0
}
```
  
The error response includes a JSON object like the following:

```shell
{
    "code": 3016,
    "message": "Can't connect database"
}
```

## Parse files in dataset

**POST** `/api/v1/dataset/{dataset_id}/chunk`

Parse files into chunks in a dataset

### Request

- Method: POST
- URL: `/api/v1/dataset/{dataset_id}/chunk`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request POST \
     --url http://{address}/api/v1/dataset/{dataset_id}/chunk \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --raw '{
         "documents": ["f6b170ac758811efa0660242ac120004", "97ad64b6759811ef9fc30242ac120004"]
     }'
```

#### Request parameters

- `"dataset_id"`: (*Path parameter*)
- `"documents"`: (*Body parameter*)
  - Documents to parse

### Response

The successful response includes a JSON object like the following:

```shell
{
    "code": 0
}
```
  
The error response includes a JSON object like the following:

```shell
{
    "code": 3016,
    "message": "Can't connect database"
}
```

## Stop file parsing

**DELETE** `/api/v1/dataset/{dataset_id}/chunk`

Stop file parsing

### Request

- Method: POST
- URL: `/api/v1/dataset/{dataset_id}/chunk`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request DELETE \
     --url http://{address}/api/v1/dataset/{dataset_id}/chunk \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --raw '{
         "documents": ["f6b170ac758811efa0660242ac120004", "97ad64b6759811ef9fc30242ac120004"]
     }'
```

#### Request parameters

- `"dataset_id"`: (*Path parameter*)
- `"documents"`: (*Body parameter*)
  - Documents to stop parsing

### Response

The successful response includes a JSON object like the following:

```shell
{
    "code": 0
}
```
  
The error response includes a JSON object like the following:

```shell
{
    "code": 3016,
    "message": "Can't connect database"
}
```

## Get document chunk list

**GET** `/api/v1/dataset/{dataset_id}/document/{document_id}/chunk`

Get document chunk list

### Request

- Method: GET
- URL: `/api/v1/dataset/{dataset_id}/document/{document_id}/chunk`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request GET \
     --url http://{address}/api/v1/dataset/{dataset_id}/document/{document_id}/chunk \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
```

#### Request parameters

- `"dataset_id"`: (*Path parameter*)
- `"document_id"`: (*Path parameter*)

### Response

The successful response includes a JSON object like the following:

```shell
{
    "code": 0
    "data": {
        "chunks": [
            {
                "available_int": 1,
                "content": "<em>advantag</em>of ragflow increas accuraci and relev:by incorpor retriev inform , ragflow can gener respons that are more accur",
                "document_keyword": "ragflow_test.txt",
                "document_id": "77df9ef4759a11ef8bdd0242ac120004",
                "id": "4ab8c77cfac1a829c8d5ed022a0808c0",
                "image_id": "",
                "important_keywords": [],
                "positions": [
                    ""
                ]
            }
        ],
        "doc": {
            "chunk_count": 5,
            "create_date": "Wed, 18 Sep 2024 08:46:16 GMT",
            "create_time": 1726649176833,
            "created_by": "134408906b6811efbcd20242ac120005",
            "id": "77df9ef4759a11ef8bdd0242ac120004",
            "knowledgebase_id": "77d9d24e759a11ef880c0242ac120004",
            "location": "ragflow_test.txt",
            "name": "ragflow_test.txt",
            "parser_config": {
                "chunk_token_count": 128,
                "delimiter": "\n!?。；！？",
                "layout_recognize": true,
                "task_page_size": 12
            },
            "parser_method": "naive",
            "process_begin_at": "Wed, 18 Sep 2024 08:46:16 GMT",
            "process_duation": 7.3213,
            "progress": 1.0,
            "progress_msg": "\nTask has been received.\nStart to parse.\nFinish parsing.\nFinished slicing files(5). Start to embedding the content.\nFinished embedding(6.16)! Start to build index!\nDone!",
            "run": "3",
            "size": 4209,
            "source_type": "local",
            "status": "1",
            "thumbnail": null,
            "token_count": 746,
            "type": "doc",
            "update_date": "Wed, 18 Sep 2024 08:46:23 GMT",
            "update_time": 1726649183321
        },
        "total": 1
    },
}
```
  
The error response includes a JSON object like the following:

```shell
{
    "code": 3016,
    "message": "Can't connect database"
}
```

## Delete document chunks

**DELETE** `/api/v1/dataset/{dataset_id}/document/{document_id}/chunk`

Delete document chunks

### Request

- Method: DELETE
- URL: `/api/v1/dataset/{dataset_id}/document/{document_id}/chunk`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request DELETE \
     --url http://{address}/api/v1/dataset/{dataset_id}/document/{document_id}/chunk \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --raw '{
         "chunks": ["f6b170ac758811efa0660242ac120004", "97ad64b6759811ef9fc30242ac120004"]
     }'
```

## Update document chunk

**PUT** `/api/v1/dataset/{dataset_id}/document/{document_id}/chunk`

Update document chunk

### Request

- Method: PUT
- URL: `/api/v1/dataset/{dataset_id}/document/{document_id}/chunk`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request PUT \
     --url http://{address}/api/v1/dataset/{dataset_id}/document/{document_id}/chunk \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --raw '{
        "chunk_id": "d87fb0b7212c15c18d0831677552d7de",  
        "knowledgebase_id": null,  
        "name": "",  
        "content": "ragflow123",  
        "important_keywords": [],   
        "document_id": "e6bbba92759511efaa900242ac120004",  
        "status": "1" 
     }'
```

## Insert document chunks

**POST** `/api/v1/dataset/{dataset_id}/document/{document_id}/chunk`

Insert document chunks

### Request

- Method: POST
- URL: `/api/v1/dataset/{dataset_id}/document/{document_id}/chunk`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request POST \
     --url http://{address}/api/v1/dataset/{dataset_id}/document/{document_id}/chunk \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --raw '{
         "document_id": "97ad64b6759811ef9fc30242ac120004",
         "content": ["ragflow content", "ragflow content"]
     }'
```

## Dataset retrieval test

**GET** `/api/v1/dataset/{dataset_id}/retrieval`

Retrieval test of a dataset

### Request

- Method: GET
- URL: `/api/v1/dataset/{dataset_id}/retrieval`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```shell
curl --request GET \
     --url http://{address}/api/v1/dataset/{dataset_id}/retrieval \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --raw '{
         "query_text": "This is a cat."
     }'
```

## Create chat

**POST** `/api/v1/chat`

Create a chat

### Request

- Method: POST
- URL: `http://{address}/api/v1/chat`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
- Body:
  - `"name"`: `string`
  - `"avatar"`: `string`
  - `"knowledgebases"`: `List[DataSet]`
  - `"id"`: `string`
  - `"llm"`: `LLM`
  - `"prompt"`: `Prompt`


#### Request example

```shell
curl --request POST \
     --url http://{address}/api/v1/chat \
     --header 'Content-Type: application/json' \
     --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
     --data-binary '{
   "knowledgebases": [
    {
      "avatar": null,
      "chunk_count": 0,
      "description": null,
      "document_count": 0,
      "embedding_model": "",
      "id": "0b2cbc8c877f11ef89070242ac120005",
      "language": "English",
      "name": "Test_assistant",
      "parse_method": "naive",
      "parser_config": {
        "pages": [
          [
            1,
            1000000
          ]
        ]
      },
      "permission": "me",
      "tenant_id": "4fb0cd625f9311efba4a0242ac120006"
    }
  ],
    "name":"new_chat_1"
}'
```

#### Request parameters

- `"name"`: (*Body parameter*)  
    The name of the created chat.  
    - `"assistant"`

- `"avatar"`: (*Body parameter*)  
    The icon of the created chat.  
    - `"path"`

- `"knowledgebases"`: (*Body parameter*)  
    Select knowledgebases associated.  
    - `["kb1"]`

- `"id"`: (*Body parameter*)  
    The id of the created chat.  
    - `""`

- `"llm"`: (*Body parameter*)  
    The LLM of the created chat.  
    - If the value is `None`, a dictionary with default values will be generated.

- `"prompt"`: (*Body parameter*)  
    The prompt of the created chat.  
    - If the value is `None`, a dictionary with default values will be generated.

---

##### Chat.LLM parameters:

- `"model_name"`: (*Body parameter*)  
    Large language chat model.  
    - If it is `None`, it will return the user's default model.

- `"temperature"`: (*Body parameter*)  
    Controls the randomness of predictions by the model. A lower temperature makes the model more confident, while a higher temperature makes it more creative and diverse.  
    - `0.1`

- `"top_p"`: (*Body parameter*)  
    Also known as "nucleus sampling," it focuses on the most likely words, cutting off the less probable ones.  
    - `0.3`

- `"presence_penalty"`: (*Body parameter*)  
    Discourages the model from repeating the same information by penalizing repeated content.  
    - `0.4`

- `"frequency_penalty"`: (*Body parameter*)  
    Reduces the model’s tendency to repeat words frequently.  
    - `0.7`

- `"max_tokens"`: (*Body parameter*)  
    Sets the maximum length of the model’s output, measured in tokens (words or pieces of words).  
    - `512`

---

##### Chat.Prompt parameters:

- `"similarity_threshold"`: (*Body parameter*)  
    Filters out chunks with similarity below this threshold.  
    - `0.2`

- `"keywords_similarity_weight"`: (*Body parameter*)  
    Weighted keywords similarity and vector cosine similarity; the sum of weights is 1.0.  
    - `0.7`

- `"top_n"`: (*Body parameter*)  
    Only the top N chunks above the similarity threshold will be fed to LLMs.  
    - `8`

- `"variables"`: (*Body parameter*)  
    Variables help with different chat strategies by filling in the 'System' part of the prompt.  
    - `[{"key": "knowledge", "optional": True}]`

- `"rerank_model"`: (*Body parameter*)  
    If empty, it uses vector cosine similarity; otherwise, it uses rerank score.  
    - `""`

- `"empty_response"`: (*Body parameter*)  
    If nothing is retrieved, this will be used as the response. Leave blank if LLM should provide its own opinion.  
    - `None`

- `"opener"`: (*Body parameter*)  
    The welcome message for clients.  
    - `"Hi! I'm your assistant, what can I do for you?"`

- `"show_quote"`: (*Body parameter*)  
    Indicates whether the source of the original text should be displayed.  
    - `True`

- `"prompt"`: (*Body parameter*)  
    Instructions for LLM to follow when answering questions, such as character design or answer length.  
    - `"You are an intelligent assistant. Please summarize the content of the knowledge base to answer the question. Please list the data in the knowledge base and answer in detail. When all knowledge base content is irrelevant to the question, your answer must include the sentence 'The answer you are looking for is not found in the knowledge base!' Answers need to consider chat history. Here is the knowledge base: {knowledge} The above is the knowledge base."`
### Response
Success:
```json
{
    "code": 0,
    "data": {
        "avatar": "",
        "create_date": "Fri, 11 Oct 2024 03:23:24 GMT",
        "create_time": 1728617004635,
        "description": "A helpful Assistant",
        "do_refer": "1",
        "id": "2ca4b22e878011ef88fe0242ac120005",
        "knowledgebases": [
            {
                "avatar": null,
                "chunk_count": 0,
                "description": null,
                "document_count": 0,
                "embedding_model": "",
                "id": "0b2cbc8c877f11ef89070242ac120005",
                "language": "English",
                "name": "Test_assistant",
                "parse_method": "naive",
                "parser_config": {
                    "pages": [
                        [
                            1,
                            1000000
                        ]
                    ]
                },
                "permission": "me",
                "tenant_id": "4fb0cd625f9311efba4a0242ac120006"
            }
        ],
        "language": "English",
        "llm": {
            "frequency_penalty": 0.7,
            "max_tokens": 512,
            "model_name": "deepseek-chat___OpenAI-API@OpenAI-API-Compatible",
            "presence_penalty": 0.4,
            "temperature": 0.1,
            "top_p": 0.3
        },
        "name": "new_chat_1",
        "prompt": {
            "empty_response": "Sorry! 知识库中未找到相关内容！",
            "keywords_similarity_weight": 0.3,
            "opener": "您好，我是您的助手小樱，长得可爱又善良，can I help you?",
            "prompt": "你是一个智能助手，请总结知识库的内容来回答问题，请列举知识库中的数据详细回答。当所有知识库内容都与问题无关时，你的回答必须包括“知识库中未找到您要的答案！”这句话。回答需要考虑聊天历史。\n            以下是知识库：\n            {knowledge}\n            以上是知识库。",
            "rerank_model": "",
            "similarity_threshold": 0.2,
            "top_n": 6,
            "variables": [
                {
                    "key": "knowledge",
                    "optional": false
                }
            ]
        },
        "prompt_type": "simple",
        "status": "1",
        "tenant_id": "69736c5e723611efb51b0242ac120007",
        "top_k": 1024,
        "update_date": "Fri, 11 Oct 2024 03:23:24 GMT",
        "update_time": 1728617004635
    }
}
```
Error:
```json
{
    "code": 102,
    "message": "Duplicated chat name in creating dataset."
}
```

## Update chat

**PUT** `/api/v1/chat/{chat_id}`

Update a chat

### Request

- Method: PUT
- URL: `http://{address}/api/v1/chat/{chat_id}`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
- Body: (Refer to the "Create chat" for the complete structure of the request body.)
  
#### Request example
```bash
curl --request PUT \
  --url http://{address}/api/v1/chat/{chat_id} \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}' \
  --data '{
    "name":"Test"
}'
```
#### Parameters
(Refer to the "Create chat" for the complete structure of the request parameters.)

### Response
Success
```json
{
    "code": 0
}
```
Error
```json
{
    "code": 102,
    "message": "Duplicated chat name in updating dataset."
}
```

## Delete chats

**DELETE** `/api/v1/chat`

Delete chats

### Request

- Method: DELETE
- URL: `http://{address}/api/v1/chat`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
- Body:
  - `ids`: List[string]
#### Request example
```bash
# Either id or name must be provided, but not both.
curl --request DELETE \
  --url http://{address}/api/v1/chat \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}' \
  --data '{
  "ids": ["test_1", "test_2"]
  }'
}'
```
#### Request parameters:

- `"ids"`: (*Body parameter*)  
    IDs of the chats to be deleted.  
    - `None`
### Response
Success
```json
{
    "code": 0
}
```
Error
```json
{
    "code": 102,
    "message": "ids are required"
}
```

## List chats

**GET** `/api/v1/chat?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id}`

List chats based on filter criteria.

### Request

- Method: GET
- URL: `http://{address}/api/v1/chat?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id}`
- Headers:
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example

```bash
curl --request GET \
  --url http://{address}/api/v1/chat?page={page}&page_size={page_size}&orderby={orderby}&desc={desc}&name={dataset_name}&id={dataset_id} \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
```

#### Request parameters
- `"page"`: (*Path parameter*)  
    The current page number to retrieve from the paginated data. This parameter determines which set of records will be fetched.  
    - `1`

- `"page_size"`: (*Path parameter*)  
    The number of records to retrieve per page. This controls how many records will be included in each page.  
    - `1024`

- `"orderby"`: (*Path parameter*)  
    The field by which the records should be sorted. This specifies the attribute or column used to order the results.  
    - `"create_time"`

- `"desc"`: (*Path parameter*)  
    A boolean flag indicating whether the sorting should be in descending order.  
    - `True`

- `"id"`: (*Path parameter*)  
    The ID of the chat to be retrieved.  
    - `None`

- `"name"`: (*Path parameter*)  
    The name of the chat to be retrieved.  
    - `None`

### Response
Success
```json
{
    "code": 0,
    "data": [
        {
            "avatar": "",
            "create_date": "Fri, 11 Oct 2024 03:23:24 GMT",
            "create_time": 1728617004635,
            "description": "A helpful Assistant",
            "do_refer": "1",
            "id": "2ca4b22e878011ef88fe0242ac120005",
            "knowledgebases": [
                {
                    "avatar": "",
                    "chunk_num": 0,
                    "create_date": "Fri, 11 Oct 2024 03:15:18 GMT",
                    "create_time": 1728616518986,
                    "created_by": "69736c5e723611efb51b0242ac120007",
                    "description": "",
                    "doc_num": 0,
                    "embd_id": "BAAI/bge-large-zh-v1.5",
                    "id": "0b2cbc8c877f11ef89070242ac120005",
                    "language": "English",
                    "name": "test_delete_chat",
                    "parser_config": {
                        "chunk_token_count": 128,
                        "delimiter": "\n!?。；！？",
                        "layout_recognize": true,
                        "task_page_size": 12
                    },
                    "parser_id": "naive",
                    "permission": "me",
                    "similarity_threshold": 0.2,
                    "status": "1",
                    "tenant_id": "69736c5e723611efb51b0242ac120007",
                    "token_num": 0,
                    "update_date": "Fri, 11 Oct 2024 04:01:31 GMT",
                    "update_time": 1728619291228,
                    "vector_similarity_weight": 0.3
                }
            ],
            "language": "English",
            "llm": {
                "frequency_penalty": 0.7,
                "max_tokens": 512,
                "model_name": "deepseek-chat___OpenAI-API@OpenAI-API-Compatible",
                "presence_penalty": 0.4,
                "temperature": 0.1,
                "top_p": 0.3
            },
            "name": "Test",
            "prompt": {
                "empty_response": "Sorry! 知识库中未找到相关内容！",
                "keywords_similarity_weight": 0.3,
                "opener": "您好，我是您的助手小樱，长得可爱又善良，can I help you?",
                "prompt": "你是一个智能助手，请总结知识库的内容来回答问题，请列举知识库中的数据详细回答。当所有知识库内容都与问题无关时，你的回答必须包括“知识库中未找到您要的答案！”这句话。回答需要考虑聊天历史。\n            以下是知识库：\n            {knowledge}\n            以上是知识库。",
                "rerank_model": "",
                "similarity_threshold": 0.2,
                "top_n": 6,
                "variables": [
                    {
                        "key": "knowledge",
                        "optional": false
                    }
                ]
            },
            "prompt_type": "simple",
            "status": "1",
            "tenant_id": "69736c5e723611efb51b0242ac120007",
            "top_k": 1024,
            "update_date": "Fri, 11 Oct 2024 03:47:58 GMT",
            "update_time": 1728618478392
        }
    ]
}
```
Error
```json
{
    "code": 102,
    "message": "The chat doesn't exist"
}
```

## Create a chat session

**POST** `/api/v1/chat/{chat_id}/session`

Create a chat session

### Request

- Method: POST
- URL: `/api/v1/chat/{chat_id}/session`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example
curl --request POST \
  --url http://{address}/api/v1/chat/{chat_id}/session \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}' \
  --data-binary '{
    "name": "new session"
  }'

## List the sessions of a chat

**GET** `/api/v1/chat/{chat_id}/session`

List all the session of a chat

### Request

- Method: GET
- URL: `/api/v1/chat/{chat_id}/session`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example
curl --request GET \
  --url http://{address}/api/v1/chat/554e96746aaa11efb06b0242ac120005/session \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

## Delete a chat session

**DELETE** `/api/v1/chat/{chat_id}/session/{session_id}`

Delete a chat session

### Request

- Method: DELETE
- URL: `/api/v1/chat/{chat_id}/session/{session_id}`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example
curl --request DELETE \
  --url http://{address}/api/v1/chat/554e96746aaa11efb06b0242ac120005/session/791aed9670ea11efbb7e0242ac120007 \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

## Update a chat session

**PUT** `/api/v1/chat/{chat_id}/session/{session_id}`

Update a chat session

### Request

- Method: PUT
- URL: `/api/v1/chat/{chat_id}/session/{session_id}`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example
curl --request PUT \
  --url http://{address}/api/v1/chat/554e96746aaa11efb06b0242ac120005/session/791aed9670ea11efbb7e0242ac120007 \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
  --data-binary '{
    "name": "Updated session"
  }'

## Chat with a chat session

**POST** `/api/v1/chat/{chat_id}/session/{session_id}/completion`

Chat with a chat session

### Request

- Method: POST
- URL: `/api/v1/chat/{chat_id}/session/{session_id}/completion`
- Headers:
  - `content-Type: application/json`
  - 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'

#### Request example
curl --request POST \
  --url http://{address}/api/v1/chat/554e96746aaa11efb06b0242ac120005/session/791aed9670ea11efbb7e0242ac120007/completion \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Bearer {YOUR_ACCESS_TOKEN}'
  --data-binary '{
    "question":  "Hello!",
    "stream": true,
  }'
