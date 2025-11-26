Acesso aos demais projetos:
- https://github.com/Pub-Class-ColinBeluco/professionalendpoint
- https://github.com/Pub-Class-ColinBeluco/uclientpoint
- https://github.com/Pub-Class-ColinBeluco/chatendpoint

Projeto de Microsserviços - Marketplace de Serviços
Este projeto demonstra a aplicação de conceitos de Arquitetura de Microsserviços, APIs REST, Docker, Testes Automatizados e Integração Contínua (CI) usando Spring Boot e GitHub Actions.O sistema é composto por dois microsserviços que se comunicam via HTTP para fornecer um catálogo de serviços para clientes e profissionais.
1. Arquitetura do Sistema
   O sistema está dividido em dois microsserviços independentes:
   | professional-service | Gerencia Profissionais, Catálogo de Serviços e Feedback. | 8080 | /api/prof, /api/serv, /api/feedback |
   | uclient-service | Gerencia Clientes (usuários) e lida com a busca de serviços. | 8080 | /api/clients |
   | chat-service | Gerencia as conversas entre clientes e profissionais | 8080 | /api/chats |
   IMPORTANTE REFORÇAR QUE OS MICROSSERVIÇOS POSSUEM SEUS SERVIDORES NO RENDER
2. Comunicação REST (Interação Obrigatória):
   O uclient-service realiza uma consulta aos dados de profissionais e serviços disponíveis no professional-service para alimentar sua funcionalidade de busca. Ambos, ao iniciar e continuar uma conversa por chat, consultam e enviam dados ao chat-service
3. Regras de Negócio
   ImplementadasBusca de Serviços (no uclient-service): Lógica de consulta a um serviço externo (professional-service) para obter a lista completa de profissionais.
   Cálculo de Avaliação Média (no professional-service): Lógica interna na classe Professional para calcular a média das notas de feedback recebidas.
4. Como Rodar Localmente (Pré-requisitos: JDK 21 e Maven)Para rodar os serviços individualmente:Clone estes repositórios.Execute os projetos separadamente no IntelliJ
5. Como Rodar com Docker
   Cada microsserviço possui seu próprio Dockerfile que utiliza um estágio multi-stage para criar uma imagem leve.
   Construir as Imagens: Na pasta raiz de cada microsserviço, execute:# Exemplo para o professional-service
docker build -t professional-service:latest .
## Exemplo para o uclient-service
docker build -t uclient-service:latest .

Executar os Containers e Criar a Rede: Para que os serviços se comuniquem, eles devem estar na mesma rede Docker.
## 1. Crie uma rede (se ainda não existir)
docker network create marketplace-net

## 2. Rode o professional-service (Serviço de Dependência)
docker run -d --name professional -p 8081:8081 --network marketplace-net professional-service:latest

## 3. Rode o uclient-service
## O Client Service DEVE usar o nome do container do outro serviço (professional) como hostname.
docker run -d --name uclient -p 8080:8080 --network marketplace-net uclient-service:latest

6. Integração Contínua (CI) com GitHub ActionsO arquivo de workflow ci.yml garante que a compilação e os testes sejam executados automaticamente em cada push para o repositório.Localização do Arquivo: .github/workflows/ci.yml8.
7. Publicação no Render
   URLs dos serviços e os endpoints:
   - uclientpoint.onrender.com/api/clients
   - chatendpoint-8r95.onrender.com/api/chats
   - professionalendpoint.onrender.com/api/prof
   - professionalendpoint.onrender.com/api/feedback
   - professionalendpoint.onrender.com/api/serv
