# Deploy de API na Nuvem com Azure DevOps

Este repositório contém um projeto que demonstra o **deploy de uma API na nuvem** utilizando **Azure DevOps** com um **agente self-hosted**. O objetivo é mostrar como automatizar a implantação de uma aplicação utilizando **pipelines personalizados**.

## 📌 Tecnologias Utilizadas

- **Linguagem**: C#/.NET 8.0
- **Azure DevOps**: Para versionamento, **CI/CD** e gerenciamento de pipeline.
- **Azure Web App**: Hospedagem da API na nuvem.
- **Agente Self-Hosted**: Para execução de jobs no pipeline sem a necessidade de um agente gerenciado.

## 🚀 Passos para Implantação

### 1. **Configuração do Agente Self-Hosted no Azure DevOps**

- No Azure DevOps, acesse o menu **Agent pools**.
- Crie um novo **pool de agentes** ou selecione um existente.
- Baixe e instale o **agente self-hosted** na máquina onde o agente será executado.
- Configure o agente utilizando o comando de configuração fornecido após o download.

### 2. **Criação do Pipeline no Azure DevOps**

1. No Azure DevOps, crie um novo pipeline.
2. Selecione o repositório onde está armazenado o código da API.
3. Defina o pipeline YAML ou utilize o assistente de configuração para criar um pipeline.

Exemplo de pipeline YAML:

```yaml
trigger:
- main

pool:
  name: Default
  demands:
    - agent.name -equals <nome-do-agente-self-hosted>

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


### 3. **Configuração do Azure Web App**

- Crie um **Azure Web App** para hospedar sua API.
- Conecte a **subscrição do Azure** ao Azure DevOps para permitir a implantação.
- No pipeline, utilize o task `AzureWebApp` para publicar a API no Web App.

### 4. **Execução do Pipeline**

Após configurar o pipeline e as etapas de build e deploy, execute o pipeline manualmente ou automaticamente com base em commits no repositório.

### 5. **Monitoramento e Logs**

- Acompanhe a execução do pipeline no Azure DevOps.
- Verifique os logs para identificar falhas e validar o sucesso do deploy.
