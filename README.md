# MULTI-STAGE BUILD
Otimização de Imagem com multi-stage-build

# Multi-Stage Dockerfile para Aplicação Go

Este repositório contém um exemplo de **Dockerfile multi-stage** para construir e executar uma aplicação escrita em Go. O objetivo é criar uma imagem final leve, utilizando uma abordagem eficiente de múltiplas etapas no Docker.

## Estrutura do Dockerfile

O Dockerfile consiste em duas abordagens:

1. **Dockerfile Simples**:
   - Usa a imagem do Go para compilar e executar a aplicação.
   - Não separa o ambiente de build e execução.

2. **Dockerfile Multi-Stage**:
   - Usa uma etapa de construção para gerar o binário da aplicação.
   - Transfere apenas o binário para uma imagem leve baseada no Alpine Linux.

## Dockerfile Explicado

### **Etapa 1: Build**
- A primeira etapa utiliza a imagem oficial do Go (`golang:1.23`) para compilar o binário.
- O código-fonte é copiado para o contêiner, e o binário é gerado.

```dockerfile
FROM golang:1.23 AS build
WORKDIR /build
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .
```

### **Etapa 2: Imagem Final**
- A segunda etapa usa a imagem base alpine:3.20.3, que é leve e ideal para produção. Apenas o binário gerado na etapa anterior é copiado para a imagem final.

```dockerfile
FROM alpine:3.20.3 AS app
WORKDIR /app
COPY --from=build /build/main .
CMD ["./main"]
```


## Vantagens do Dockerfile Multi-Stage

1. **Imagens Mais Leves**:
   - Apenas o binário necessário é incluído na imagem final.

2. **Ambiente Isolado**:
   - A etapa de construção usa a imagem completa do Go, mas a execução utiliza uma imagem mínima (Alpine).

3. **Segurança Melhorada**:
   - Reduz a superfície de ataque ao remover ferramentas desnecessárias na imagem final.


![image](https://github.com/user-attachments/assets/8b95e2d9-60d6-46bf-be2f-aad16daa4742)



![image](https://github.com/user-attachments/assets/d4cd3852-dbf5-465e-8f94-5021ec1524b4)



![image](https://github.com/user-attachments/assets/b69f4c93-563d-4800-9397-f976cd3c7830)





