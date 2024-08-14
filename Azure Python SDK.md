# Azure storage account 
- https://www.youtube.com/watch?v=gMG3JQpyhTo
- connect string
  - storage account/ Access Keys - Connection string
  - 
> pip install azure-storage-blob azure-identity
> 
```py
import os
from azure.storage.blob import BlobServiceClient

storage_connection_string = "copy from connection string from above"
blob_service_client = BlobServiceClient.from_connection_string(storage_connection_string)

# create a container
container_name = "Image"
container_client  = blob_service_client.create_container(container_namem, public_access="OFF")

# upload files from my computer
file_folder = './files'
for file_name in os.listdir(file_folder):
    blob_obj = blob_service_client.get_blob_client(container = container_name, blob = file_name)
    print(f"Uploading file :{file_name} ..."

    with open(os.path.join(file_folder, file_name), mode = 'rb') as file_data:
        blob_obj.upload_blob(file_data)
    

```
