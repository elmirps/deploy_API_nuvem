# Deploy de API na Nuvem com Azure DevOps

Este repositório contém um projeto que demonstra o **deploy de uma API na nuvem** utilizando **Azure DevOps** com um **web hook**. O objetivo é mostrar como automatizar a implantação de uma aplicação utilizando **pipelines personalizados**.

## 📌 Tecnologias Utilizadas

- **Linguagem**: C#/.NET 8.0
- **Azure DevOps**: Para **versionamento** e **gerenciamento de pipeline**.
- **Azure Web App**: Hospedagem da API na nuvem.
- **Web hook**: Para execução de **jobs** no pipeline sem necessidade de agente gerenciado.

## 🚀 Passos para Implantação

### 1. **Configuração do Web Hook**

1. No **Azure DevOps**, acesse o projeto e configure um **web hook** para se conectar aos eventos do pipeline.
2. Defina um **endpoint** onde o web hook irá enviar notificações quando um evento de build ou release for disparado.

### 2. **Criação do Pipeline no Azure DevOps**

1. Crie um novo **pipeline** no Azure DevOps.
2. Selecione o repositório onde está armazenado o código da API.
3. Utilize **YAML** ou o assistente de configuração para criar o pipeline.

Exemplo de pipeline YAML com **web hook**:

```yaml
trigger:
- main

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'

jobs:
- job: BuildAndDeploy
  steps:
  - task: UseDotNet@2
    inputs:
      packageType: 'sdk'
      version: '8.x'
  - task: Restore@2
  - task: Build@1
  - task: Publish@1
  - task: AzureWebApp@1
    inputs:
      azureSubscription: '<nome-da-subscrição-azure>'
      appName: '<nome-da-sua-api>'
      package: '$(System.DefaultWorkingDirectory)/**/*.zip'
  - task: WebHook@2
    inputs:
      url: '<seu-endpoint-webhook>'
      method: 'POST'
      data: '{"status": "completed"}'


### 3. **Configuração do Azure Web App**

- Crie um **Azure Web App** para hospedar sua API.
- Conecte a **subscrição do Azure** ao Azure DevOps para permitir a implantação.
- No pipeline, utilize o task `AzureWebApp` para publicar a API no Web App.

### 4. **Execução do Pipeline**

Após configurar o pipeline e as etapas de build e deploy, execute o pipeline manualmente ou automaticamente com base em commits no repositório.

### 5. **Monitoramento e Logs**

- Acompanhe a execução do pipeline no Azure DevOps.
- Verifique os logs para identificar falhas e validar o sucesso do deploy.
