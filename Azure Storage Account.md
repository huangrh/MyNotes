
## Reference:  
- [R interface to Azure Storage Services](https://github.com/Azure/AzureStor)
- [Manage blobs with Python v12 SDK](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python?tabs=environment-variable-windows)
- [Use Python to manage directories and files in Azure Data Lake Storage Gen2(ADLS Gen2)](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-directory-file-acl-python)
```
storage_account_key = access_key
def initialize_storage_account(storage_account_name, storage_account_key):
    
    try:  
        global service_client

        service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
            "https", storage_account_name), credential=storage_account_key)
    
    except Exception as e:
        print(e)
```

- [ADLS Gen2 - AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-blobs-copy?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json)

- https://learn.microsoft.com/en-us/azure/storage/common/storage-ref-azcopy-copy?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json

- https://stackoverflow.com/questions/69807720/connect-excel-to-azure-datalake-gen-2-with-oauth
