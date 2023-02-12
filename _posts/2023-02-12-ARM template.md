---
layout: post
title: "ARM template"
date: 2023-02-12
---

Notes for Azure ARM templates

# Introduction 

这是一套用于快速部署Azure服务的硬核Json格式参数。

## Install

- VS code：微软在vs code上集成一款辅助手写参数的脚本，在market搜索ARM，安装这款辅助插件
- Powershell: 安装Azure Cli，以及相关插件。
      - https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell

## QuickStart



- 创建空白json文件
- 敲入`arm`, 按提示自动填充参数模板，如下所示

```
{
    "$schema": ""
    "contentVersion": ""
    "parameters": {}
    "functions": []
    "variables": {}
    "resources": []
    "outputs": {}
}
```

- resources内定义资源

```
"resources": [
    {
      "type": "Microsoft.Storage/storageAccounts", // 资源类别 namespace/type
      "apiVersion": "2021-09-01",                  // API 版本
      "name": "{provide-unique-name}",
      "location": "eastus",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2",
      "properties": {
        "supportsHttpsTrafficOnly": true
      }
    }
  ]
```

- parameters为自定义参数，用于定义资源名称等，可以在resources内引用。如参数太多，可右键鼠标选择 create parameter file，创建独立的文档。
  - 支持customize

```
"parameters": {
    "storageName": {
      "type": "string",
      "minLength": 3,
      "maxLength": 24
    },
    "storageSKU": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Standard_ZRS",
        "Premium_LRS",
        "Premium_ZRS",
        "Standard_GZRS",
        "Standard_RAGZRS"
      ]
    }
  },
```

- veriable: 使用function生成自定义字符串
  
```
{
    "uniqueStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]"
  },
```

# Deploy

Powershell快速部署

```
New-AzResourceGroup -Name arm-vscode -Location eastus

New-AzResourceGroupDeployment -ResourceGroupName arm-vscode -TemplateFile ./azuredeploy.json -TemplateParameterFile ./azuredeploy.parameters.json
```

删除资源

```
Remove-AzResourceGroup -Name arm-vscode
```

# 导出template

用于获取已有资源的template。从资源页面选择 export template，导出Json文件。

# Quick Example

https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-quickstart-template?tabs=azure-powershell
https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager

- Web App

  - https://github.com/Azure/azure-quickstart-templates/blob/master/quickstarts/microsoft.web/webapp-basic-linux/azuredeploy.json




# Reference

https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview
