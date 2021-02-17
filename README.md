# Assignment - Service Design & Implementation

## Scenario

You are now an employee of the company, and currently we use Amazon Web Service  to hold our systems. As a senior backend engineer, your first mission is to design and implement a new flow for one of our daily data processes since the current one takes a long time to finish.

New raw data will be restored in the `S3` bucket `everyday at 4 A.M`. in `.xlsx` format, each file contains at least `500,000` records and there will be at least `1000` files in one day.

Your goal is to `design the ETL process` with these data and retrieve these data via `API service` eventually.  

## Prerequisite

* You can only use cloud services to design your architecture, e.g. `Amazon Web Service`, `Microsoft Azure`, `Google Cloud Platform`.

* File name of raw data > `{aws-account}-{yyyy-MM-dd}-raw.xlsx`
  * e.g. `987654321-2021-01-02-raw.xlsx`.

* There will be an API server to handle the call from other systems
* Use the `.csv` file in `data` folder as processed data, which will be restored in database.

## Target

1. Design and draw architecture for the data process and show where can API server retrieve the processed data.
2. Design and draw table schema. Use the `.csv` file in `data` folder, your table __MUST__ have the following columns down below.
      | Column | Description |
      | -- | -- |
      | bill/PayerAccountId | The account ID of the paying account. For an organization in AWS Organizations, this is the account ID of the master account. |
      |lineItem/UnblendedCost | The UnblendedCost is the UnblendedRate multiplied by the UsageAmount. |
      | lineItem/UnblendedRate | The uncombined rate for specific usage. For line items that have an RI discount applied to them, the UnblendedRate is zero. Line items with an RI discount have a UsageType of Discounted Usage. |
      | lineItem/UsageAccountId |The ID of the account that used this line item. For organizations, this can be either the master account or a member account. You can use this field to track costs or usage by account. |
      | lineItem/UsageAmount | The amount of usage that you incurred during the specified time period. For size-flexible reserved instances, use the reservation/TotalReservedUnits column instead. |
      | lineItem/UsageStartDate |  The start date and time for the line item in UTC, inclusive. The format is YYYY-MM-DDTHH:mm:ssZ. |
      | lineItem/UsageEndDate | The end date and time for the corresponding line item in UTC, exclusive. The format is YYYY-MM-DDTHH:mm:ssZ. |
      | product/ProductName | The full name of the AWS service. Use this column to filter AWS usage by AWS service. Sample values: AWS Backup, AWS Config, Amazon Registrar, Amazon Elastic File System, Amazon Elastic Compute Cloud |
3. Design 2 APIs with the following requirements 
    1. Get __lineItem/UnblendedCost__ grouping by __product/productname__
        - Input
          | Column | Required |
          | ------ | -------- |
          | lineitem/usageaccountid | true |
        - Output 
        ```JSON
            {
                "{product/productname_A}": "sum(lineitem/unblendedcost)",
                "{product/productname_B}": "sum(lineitem/unblendedcost)",
                ...
            }
        ```
    2. Get daily __lineItem/UsageAmount__ grouping by __product/productname__
        - Input
          | Column | Required |
          | ------ | -------- |
          | lineitem/usageaccountid | true |
        - Output
        ```JSON
            {
                "{product/productname_A}": {
                    "YYYY/MM/01": "sum(lineItem/UsageAmount)",
                    "YYYY/MM/02": "sum(lineItem/UsageAmount)",
                    "YYYY/MM/03": "sum(lineItem/UsageAmount)",
                    ...
                },
                "{product/productname_B}": {
                    "YYYY/MM/01": "sum(lineItem/UsageAmount)",
                    "YYYY/MM/02": "sum(lineItem/UsageAmount)",
                    "YYYY/MM/03": "sum(lineItem/UsageAmount)",
                    ...
                },
            }
        ```
4. Your can only use __Python__ or __C#__ to implement APIs.
5. *(Optional)* Use any mechanism to reduce the process time during data process flow.
6. *(Optional)* You __can add more input arguments__ to your APIs to achieve paginaton strategy.
7. *(Optional)* Make the API service to be container-based.
8. *(Optional)* Use any mechanism to reduce response time when calling the APIs, which can be algorithm or caching.

## Deliverable

1. Upload codes to your __GitHub__ and __provide repo URL__.
2. Include a __README.md__ file in __the root of repo__.
3. Provide your __Architecture__ and describe how it works to __README.md__.
4. Provide your __API spec__, __API URLs__, and __DB schema__ to __README.md__.
5. Use any kind of framework, e.g. __ASP.NET 5__, __Django__, __Flask__.
6. *(Optional)* Describe how you reduce the response time / improve performance to __README.md__.

## Notice

* Once you finished the assignment, send an email back to the one who contacted you.
* Leave comments to __README.md__ if there is any.
* You only need to implement the API part.
