# Observability Stack

Este projeto é uma _stack_ modular de **observabilidade** que integra as principais ferramentas open source para monitoramento, coleta de métricas, logs e alertas. A ideia central é oferecer uma solução rápida e prática para instrumentar aplicações, correlacionando dados de performance, saúde e comportamento das aplicações em tempo real.

## Visão Geral

A _Observability Stack_ foi desenvolvida para:

- **Monitorar e visualizar métricas**: Utilizando o Prometheus para coleta e armazenamento de métricas e o Grafana para dashboards interativos e análises detalhadas.
- **Expor dados operacionais da aplicação**: Através de endpoints _actuator_ (ou integração com Micrometer), que fornecem informações sobre a saúde, performance e métricas customizadas da aplicação.
- **Centralizar alertas**: Com o Alertmanager (ou outra ferramenta de alerta) para notificar problemas e facilitar a resolução de incidentes.

A estrutura do repositório está organizada em pastas para cada componente do stack, como _prometheus_, _grafana_, _nginx_, _app_ (a aplicação exemplo) e outros serviços auxiliares.

## Arquitetura

A arquitetura da _Observability Stack_ integra os seguintes componentes:

- **Aplicação Instrumentada**: Uma aplicação (localizada na pasta `app`) que expõe endpoints _actuator_ com integração ao Micrometer, permitindo a coleta de métricas (por exemplo, em `/actuator/prometheus`).
- **Prometheus**: Responsável por fazer _scrape_ dos endpoints de métricas expostos pela aplicação e pelos demais serviços. As configurações personalizadas encontram-se na pasta `prometheus`.
- **Grafana**: Ferramenta de visualização que se conecta ao Prometheus e possibilita a criação de dashboards para análise dos dados coletados. Configurações e dashboards podem ser encontrados na pasta `grafana`.
- **Alertmanager**: (Opcional) Para gerenciamento de alertas e notificação de incidentes.
- **Nginx**: Usado como proxy reverso para direcionar e balancear as requisições entre os serviços.
- **Outros serviços**: Como banco de dados (ex.: `mysql`) e clientes de teste (diretório `client`) para simulação de carga e verificação do comportamento da aplicação.

> **Nota:** A integração entre os endpoints _actuator_ e a coleta de métricas via Micrometer é fundamental para fornecer insights precisos sobre o desempenho da aplicação. Essa integração permite que o Prometheus acesse dados em tempo real, enquanto o Grafana exibe esses dados em dashboards customizados.

## Como Funciona a Observabilidade

1. **Instrumentação da Aplicação**  
   A aplicação utiliza os _actuator endpoints_ (e Micrometer, se for um ambiente Spring Boot ou similar) para expor métricas de performance, status de saúde e métricas customizadas. Esses endpoints geralmente ficam disponíveis em uma rota como `/actuator/prometheus`.

2. **Coleta de Métricas com Prometheus**  
   O Prometheus é configurado para fazer _scrape_ nos endpoints da aplicação e em outros serviços do stack. Essa coleta periódica possibilita a construção de uma base histórica de métricas.

3. **Visualização com Grafana**  
   O Grafana conecta-se ao Prometheus (e, se necessário, a outras fontes) para criar dashboards dinâmicos e interativos. Isso permite identificar rapidamente padrões, anomalias e correlacionar métricas com logs e alertas.

4. **Alertas e Notificações**  
   Com o Alertmanager integrado, é possível configurar regras de alerta baseadas em thresholds definidos para as métricas, facilitando a detecção e resposta a incidentes em tempo real.

## Pré-requisitos

- **Docker** e **Docker Compose** instalados na máquina.
- (Opcional) Para executar a aplicação localmente com suporte à instrumentação, certifique-se de que o ambiente está preparado para compilar e rodar a aplicação (por exemplo, JDK, Maven/Gradle, se for um projeto Java).

## Instalação e Execução

1. **Clone o repositório**:

   ```bash
   git clone https://github.com/Patrick-Augusto/observability-stack.git
   cd observability-stack
   ```

2. **Suba a stack com o Docker Compose**:

   ```bash
   docker-compose up --build
   ```

   Este comando irá inicializar todos os serviços configurados, incluindo Prometheus, Grafana, a aplicação instrumentada e demais componentes.

3. **Acesse o Grafana**:

   Abra o navegador e acesse [http://localhost:3000](http://localhost:3000). Utilize as credenciais padrão (se configuradas) para visualizar os dashboards de observabilidade.

4. **Verifique os Endpoints Actuator**:

   Acesse o endpoint da aplicação, por exemplo, [http://localhost:8080/actuator/prometheus](http://localhost:8080/actuator/prometheus), para confirmar que as métricas estão sendo expostas corretamente.

## Configurações e Personalizações

- **Docker Compose**: O arquivo `docker-compose.yaml` contém a orquestração dos serviços. É possível ajustar variáveis de ambiente, portas e volumes conforme necessário.
- **Prometheus**: A pasta `prometheus` inclui o arquivo de configuração (`prometheus.yml`) que define os targets para scraping.
- **Grafana**: Dashboards e fontes de dados podem ser importados ou modificados a partir da interface do Grafana. Verifique a pasta `grafana` para arquivos JSON de dashboards.
- **Actuator/Micrometer**: Caso seja necessário customizar a instrumentação da aplicação, ajuste as configurações no projeto da aplicação (por exemplo, propriedades do Spring Boot).

## Exemplos de Uso

- **Monitoramento em Tempo Real**: Visualize a performance e a saúde da aplicação por meio dos dashboards do Grafana.
- **Análise de Incidentes**: Utilize os alertas configurados no Prometheus e no Alertmanager para identificar e responder rapidamente a falhas.
- **Correlações**: Relacione métricas de performance com logs e traces para identificar gargalos e pontos de falha na aplicação.

## Contribuições

Contribuições são bem-vindas! Se você deseja melhorar a _Observability Stack_ ou adicionar novos recursos, por favor, abra uma _issue_ ou submeta um _pull request_.

---

Esta _Observability Stack_ visa ser uma ferramenta prática e flexível para desenvolvedores e equipes de operações, facilitando o monitoramento e a análise do comportamento de aplicações em ambientes de teste e produção.

---

Caso tenha dúvidas ou sugestões, sinta-se à vontade para abrir uma _issue_ no repositório.

---

Este README pode ser ajustado conforme a evolução do projeto e a integração de novos componentes de observabilidade.

---

*Fontes e Inspirações:*  
- [Grafana](https://grafana.com)  
- [Prometheus](https://prometheus.io)  
- [Spring Boot Actuator & Micrometer](https://spring.io/projects/spring-boot)  
