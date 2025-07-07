# teste-primeiro-projeto-https-mulesoft

---

## Como criar o primeiro projeto MuleSoft "HTTPS" para iniciantes (passo a passo)
Tem o objetivo orientar desenvolvedores iniciantes na criação de um serviço HTTPS utilizando a plataforma MuleSoft, especificamente com o Anypoint Studio.

---

## Introdução ao HTTPS no MuleSoft

O protocolo HTTPS é essencial para garantir segurança na troca de informações entre clientes e serviços. No MuleSoft, configurar um endpoint HTTPS envolve ajustes específicos no conector HTTP Listener e na definição de certificados SSL.

---

## Pré-requisitos

Antes de começar, certifique-se de:

- Ter o **Anypoint Studio** instalado  
- Possuir um **certificado SSL** (autoassinado ou emitido por uma autoridade)  
- Ter conhecimentos básicos de **Mule Flows** e configuração de conectores

---

## Etapas para Criar um Serviço HTTPS

### 1. Criar um Novo Projeto Mule  
- Abra o **Anypoint Studio**  
- Selecione **File > New > Mule Project**  
- Nomeie seu projeto (ex: `https-service`)  

### 2. Configurar o Listener HTTP  
- Na aba de paleta, arraste um **HTTP Listener** para o canvas  
- Clique em `+` para criar uma nova configuração de listener  
- Selecione o protocolo como **HTTPS**

### 3. Gerar ou Importar um Certificado SSL  
- Você pode utilizar o comando Java `keytool` para gerar um certificado autoassinado:  
  ```bash
  keytool -genkeypair -alias mule_cert -keyalg RSA -keystore keystore.jks -storepass senha123
  ```
- Importe o `keystore.jks` para seu projeto

### 4. Configurar o Keystore na Listener Configuration  
- Configure os seguintes campos:
  - **Keystore Path**: caminho do arquivo `.jks`
  - **Keystore Password**: senha definida
  - **Key Alias**: alias do certificado (não obrigatório)
  - **Key Password**: senha da chave

### 5. Definir a Porta HTTPS  
- Escolha uma porta segura, como `8443`  
- Aponte o listener para: `https://localhost:8443`

### 6. Adicionar um Processador de Fluxo  
- Após o listener, adicione um componente de lógica, como um **Logger** ou **Transform Message**

### 7. Executar e Testar  
- Inicie seu projeto no Mule Runtime  
- Acesse via navegador ou ferramenta como Postman:  
  ```
  https://localhost:8443/teste-primeiro-projeto-https-mulesoft
  ```

---

## Dicas e Boas Práticas

- Utilize certificados válidos em ambientes de produção  
- Evite expor informações sensíveis nos logs  
- Faça testes com ferramentas como **OpenSSL**, **Postman** e **Curl**

---

## Configuração no CloudHub
A criação de um serviço HTTPS no CloudHub difere em alguns aspectos importantes do processo no Anypoint Studio. Vamos comparar os dois ambientes passo a passo:

---

## Diferenças entre CloudHub e Anypoint Studio na Configuração de HTTPS
| Aspecto | Anypoint Studio (Local) | CloudHub (Implantação na Nuvem) | 
| Ambiente | Local, usado para desenvolvimento e testes | Nuvem, usado para produção e escalabilidade | 
| Configuração de HTTPS | Manual via HTTP Listener + Keystore | Automática via domínio *.cloudhub.io com TLS gerenciado | 
| Certificado SSL | Gerado e configurado pelo desenvolvedor (.jks) | Gerenciado pela MuleSoft (CloudHub usa certificados válidos) | 
| Porta HTTPS | Definida pelo usuário (ex: 8443) | Padrão 443 (HTTPS) | 
| Domínio de acesso | https://localhost:8443 | https://nome-app.cloudhub.io | 
| Segurança adicional | Pode configurar mTLS manualmente | Suporte nativo a mTLS e IP whitelisting via Anypoint Security | 

---

## Como Criar um Serviço HTTPS no CloudHub
1. Criar o Projeto no Anypoint Studio
- Desenvolva seu fluxo normalmente com um HTTP Listener
- Use o protocolo HTTP (não precisa configurar HTTPS localmente)

2. Implantar no CloudHub
- Clique com o botão direito no projeto > Deploy to CloudHub
- Escolha o nome do aplicativo (ex: meu-servico)
- O CloudHub automaticamente expõe o endpoint como:
https://meu-servico.cloudhub.io

3. Testar o Endpoint HTTPS
- Use Postman, Curl ou navegador para acessar o serviço
- O certificado TLS é gerenciado pela MuleSoft, então você já está protegido

---

## E se eu quiser usar mTLS ou certificados personalizados?
Pode-se utilizar o Anypoint Security, que permite:
- Substituir o certificado padrão por um personalizado
- Configurar autenticação mTLS
- Definir políticas de segurança avançadas

