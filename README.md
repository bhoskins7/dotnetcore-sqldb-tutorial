---
languages:
- csharp
- aspx-csharp
page_type: sample
description: "This is a sample application you can use to follow along w/ the Build a .NET Core and SQL Database web app in Azure Web Apps for Containers tutorial."
products:
- azure
- aspnet-core
- azure-app-service
---

# .NET Core MVC sample for Azure App Service

This is a sample application that you can use to follow along with the tutorial at 
[Build a .NET Core and SQL Database web app in Azure Web Apps for Containers](https://docs.microsoft.com/azure/app-service/containers/tutorial-dotnetcore-sqldb-app). 

## License

See [LICENSE](LICENSE.md).

## Contributing

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.



az sql server create --name todo-db --resource-group a-test --location westus2 --admin-user elasticdotnet --admin-password sdflkjj234#1
az sql server firewall-rule create --resource-group a-test --server todo-db --name AllowAzureIps --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
az sql server firewall-rule create --name AllowLocalClient --server todo-db --resource-group a-test --start-ip-address 97.91.102.124 --end-ip-address 97.91.102.124
az sql db create --resource-group a-test --server todo-db --name coreDB --service-objective S0
az sql db show-connection-string --client ado.net --server todo-db --name coreDB
"Server=tcp:todo-db.database.windows.net,1433;Database=coreDB;User ID=elasticdotnet;Password=sdflkjj234#1;Encrypt=true;Connection Timeout=30;"

rm -r Migration
dotnet ef migrations add InitialCreate
$env:ConnectionStrings:MyDbConnection="Server=tcp:todo-db.database.windows.net,1433;Database=coreDB;User ID=elasticdotnet;Password=sdflkjj234#1;Encrypt=true;Connection Timeout=30;"
set ConnectionStrings:MyDbConnection="Server=tcp:todo-db.database.windows.net,1433;Database=coreDB;User ID=elasticdotnet;Password=sdflkjj234#1;Encrypt=true;Connection Timeout=30;"


"KibanaAirlines.count": {
      "filter": {
        "term": {
          "Carrier": "Kibana Airlines"
        }
      }
    },
    "LogstashAirways.count": {
      "filter": {
        "term": {
          "Carrier": "Logstash Airways"
        }
      }
    },


    {
  "group_by": {
    "timestamp": {
      "date_histogram": {
        "field": "timestamp",
        "calendar_interval": "1h"
      }
    }
  },
  "aggregations"{
    "LogstashAirways.count": {
      "filter": {"term": {"Carrier": "Logstash Airways"}}
    },
     "KibanaAirlines.count": {
      "filter": { "term": { "Carrier": "Kibana Airlines"}}
    },
    "time_length": { 
      "bucket_script": {
         "buckets_path": { 
           "min": "LogstashAirways.count.value",
           "max": "KibanaAirlines.count.value"
            },
          "script": "params.max - params.min" 
        }
      }
    }
  }
}

{
  "group_by": {
    "timestamp": {
      "date_histogram": {
        "field": "timestamp",
        "calendar_interval": "1h"
      }
    }
  },
  "aggregations": {
    "LogstashAirways": {
      "filter": {
        "term": {
          "Carrier": "Logstash Airways"
        }
      },
      "aggs": {
        "Carrier.value_count": {
          "value_count": {
            "field": "Carrier"
          }
        }
      }
    },
    "JetBeats": {
      "filter": {
        "term": {
          "Carrier": "JetBeats"
        }
      },
      "aggs": {
        "Carrier.value_count": {
          "value_count": {
            "field": "Carrier"
          }
        }
      }
    },
    "bucket_script": {
    "buckets_path": {
      "my_var1": "JetBeats.Carrier.value_count.value",                     
      "my_var2": "Logstash Airways"
    },
    "script": "params.my_var1 / params.my_var2"
  }
  }
}