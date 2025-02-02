# Dockerizando uma Aplicacao Java Spring Boot

![Docker e Java](A_visually_striking_image_combining_Docker_and_Jav.png)

Este repositório contém um `Dockerfile` para criar uma imagem Docker de uma aplicação Java Spring Boot. O processo está dividido em duas fases: compilação e execução.

## Estrutura do Dockerfile

O `Dockerfile` segue uma abordagem multi-stage para otimizar a imagem final:

### 1. Fase de Build (Compilação do Projeto)
```dockerfile
FROM maven:3.8.5-openjdk-17-slim AS build
```
Utiliza a imagem oficial do Maven com o OpenJDK 17 para compilar o projeto.

```dockerfile
WORKDIR /app
COPY . .
```
Define o diretório de trabalho como `/app` e copia todo o conteúdo do projeto para dentro do container.

```dockerfile
RUN mvn clean package -DskipTests
```
Executa o Maven para compilar o projeto e gerar o artefato `.jar`, ignorando os testes para acelerar o build.

### 2. Fase de Execução (Imagem Final)
```dockerfile
FROM openjdk:17-jdk-slim
```
Usa uma imagem mais leve do OpenJDK 17 para executar a aplicação, reduzindo o tamanho final da imagem.

```dockerfile
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
```
Copia o `.jar` gerado na fase de build para a imagem final.

```dockerfile
EXPOSE 8080
```
Expõe a porta 8080, usada pela aplicação Spring Boot.

```dockerfile
ENTRYPOINT ["java", "-jar", "app.jar"]
```
Define o comando padrão para iniciar a aplicação Java.

## Como Construir e Rodar o Container

### Passo 1: Construir a Imagem Docker
Execute o comando abaixo no diretório raiz do projeto:
```sh
docker build -t minha-aplicacao .
```
Isso criará uma imagem chamada `minha-aplicacao`.

### Passo 2: Rodar o Container
Para iniciar o container e mapear a porta 8080 para acesso local:
```sh
docker run -p 8080:8080 minha-aplicacao
```
Agora a aplicação estará acessível via `http://localhost:8080`.

## Conclusão
Este `Dockerfile` utiliza a abordagem multi-stage para reduzir o tamanho da imagem final e garantir um ambiente otimizado para execução da aplicação Java Spring Boot. Seguindo os passos acima, você pode facilmente construir e rodar sua aplicação dentro de um container Docker.

