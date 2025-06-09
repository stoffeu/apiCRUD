# Estágio de Build
# Usa uma imagem base do Maven com o JDK 17 para compilar o projeto
FROM maven:3.8.5-openjdk-17 AS build

# Define o diretório de trabalho dentro do container
WORKDIR /app

# Copia o arquivo de configuração do Maven
COPY pom.xml .

# Baixa as dependências do projeto
RUN mvn dependency:go-offline

# Copia todo o código fonte do projeto
COPY src ./src

# Compila o projeto e gera o arquivo .jar
RUN mvn package -DskipTests

# Estágio de Execução
# Usa uma imagem base mais leve, apenas com o OpenJDK 17, para rodar a aplicação
FROM openjdk:17-jdk-slim

# Define o diretório de trabalho
WORKDIR /app

# Copia o arquivo .jar gerado no estágio de build para o novo container
COPY --from=build /app/target/crud-api-0.0.1-SNAPSHOT.jar app.jar

# Expõe a porta 8080, que é a porta padrão do Spring Boot
EXPOSE 8080

# Comando para executar a aplicação quando o container iniciar
ENTRYPOINT ["java", "-jar", "app.jar"]