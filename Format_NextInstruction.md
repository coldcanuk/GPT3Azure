Return to main - [GPT3Azure](README.md)
# Next Instruction prompt format with JSON output
>Using Microsoft Azure and Microsoft Visual Studio Code create an application with Azure Logic Apps and Azure Blob Storage. Develop using Powershell when possible otherwise develop using Python. 
Please create the the Azure Blob Storage. Please create a new Azure Logic App and title it MyNewLogicApp. Follow the instructions below.\
Next Instruction# Create a new Azure Blob storage.\
Next Instruction# Create a new Azure Logic App and title it MyNewLogicApp.\
Next Instruction# MyNewLogicApp will trigger When a HTTP request is received. \
Next Instruction# Label this trigger as inHTTP.\
Next Instruction# inHTTP will use the following Request Body JSON Schema { "properties": { "items": { "type": "string" } }, "type": "object" }\
Next Instruction# Check and verify that the content-type is JSON\
Next Instruction# Check and verify that the API KEY has a value of HammerTime\
Next Instruction# MyNewLogicApp will create a new condition and label it Check4Key\
Next Instruction# Check4Key uses an IF THEN ELSE logic\
Next Instruction# IF inHTTP does not contain the API KEY; THEN Check4Key will return to the requestor a standard 403 forbidden message AND terminate the MyNewLogicApp \
Next Instruction# IF inHTTP does contain the API KEY; THEN Continue\
Next Instruction# INJECT RESTART TEXT\
Action#Create a new Azure Blob storage\
\$storageAccount = New-AzStorageAccount -ResourceGroupName myResourceGroup -Name myStorageAccount -SkuName Standard_LRS -Location westus\
Action#Create a new Azure Logic App\
\$logicApp = New-AzLogicApp -ResourceGroupName myResourceGroup -Name MyNewLogicApp -Location westus -Sku S1\

## Codex Generated Output below *Currently untested*
>\$logicApp = New-AzLogicApp -ResourceGroupName myResourceGroup -Name MyNewLogicApp -Location westus -Sku S1 -Workflow @"
```
{
  '$schema': 'https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#',
  'contentVersion': '1.0.0.0',
  'parameters': {
    'items': {
      'type': 'string'
    }
  },
  'triggers': {
    'inHTTP': {
      'type': 'http',
      'schema': {
        'type': 'object',
        'properties': {
          'items': {
            'type': 'string'
          }
        },
        'required': [
          'items'
        ]
      },
      'method': 'post',
      'url': 'https://mystorageaccount.blob.core.windows.net/mycontainer/myblob',
      'headers': {
        'content-type': 'application/json'
      },
      'body': '{ "items": \$input.json(\'\$.items\') }'
    }
  },
  'actions': {
    'Restart': {
      'type': 'restart'
    }
  },
  'triggers': {
    'Check4Key': {
      'type': 'conditional',
      'condition': 'not contains(azure-asyncoperation, "does not exist")',
      'trueAction': 'Restart',
      'falseAction': 'error'
    }
  }
}
```
>Next Instruction#